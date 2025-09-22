# Aleph Vault

The **Aleph Vault** is an enhanced [ERC-7540](https://eips.ethereum.org/EIPS/eip-7540) compatible smart contract that enables users to participate in off-chain strategies by asynchronously depositing and redeeming on-chain ERC-20 assets. It uses **batch-based settlements**, where an oracle periodically finalizes deposits and redemptions based on updated Net Asset Value (NAV).

#### Vault Configuration

Each Aleph Vault is initialized with comprehensive parameters structured in three main categories:

**Core Parameters:**
* `operationsMultisig`: Aleph protocol operations multisig for administrative controls
* `vaultFactory`: The factory contract that deployed this vault
* `oracle`: The trusted oracle for NAV updates and settlement operations
* `guardian`: Guardian role for emergency pause capabilities
* `authSigner`: Off-chain KYC attestation signer
* `accountant`: The accountant contract managing fee collection and distribution
* `batchDuration`: The fixed duration of each settlement batch (e.g., hourly, daily)

**User-Provided Parameters:**
* `name`: The name of the vault
* `configId`: Vault configuration identifier
* `manager`: Manager role for day-to-day vault operations
* `underlyingToken`: The ERC-20 token the vault accepts
* `custodian`: The custodian address where deposited funds are held
* `vaultTreasury`: Treasury address for vault's portion of collected fees
* `shareClassParams`: Parameters for the initial share class (fees, limits, periods)

**Module Implementations:**
* `alephVaultDepositImplementation`: Deposit logic module
* `alephVaultRedeemImplementation`: Redemption logic module
* `alephVaultSettlementImplementation`: Settlement logic module
* `feeManagerImplementation`: Fee management module
* `migrationManagerImplementation`: Migration and upgrade module

### Write Methods

#### setIsDepositAuthEnabled

```solidity
function setIsDepositAuthEnabled(bool _isDepositAuthEnabled) external;
```

Toggle KYC gating for deposits. When enabled, off-chain attestations from `authSigner` are enforced for deposit requests.

**Emits:**

* `IsDepositAuthEnabledSet(bool isDepositAuthEnabled)`

#### setIsSettlementAuthEnabled

```solidity
function setIsSettlementAuthEnabled(bool _isSettlementAuthEnabled) external;
```

Toggle KYC gating for settlements. When enabled, off-chain attestations from `authSigner` are enforced for settlement operations.

**Emits:**

* `IsSettlementAuthEnabledSet(bool isSettlementAuthEnabled)`

#### createShareClass

```solidity
function createShareClass(
    ShareClassParams memory _shareClassParams
) external returns (uint8 _classId);
```

Create a new **share class** with its own fee schedule, periods, and limits. Returns the new `classId`.

**ShareClassParams structure:**
* `managementFee`: Management fee rate in basis points
* `performanceFee`: Performance fee rate in basis points
* `noticePeriod`: Notice period in batches for redemptions
* `lockInPeriod`: Lock-in period in batches
* `minDepositAmount`: Minimum deposit amount
* `minUserBalance`: Minimum user balance requirement
* `maxDepositCap`: Maximum deposit cap
* `minRedeemAmount`: Minimum redemption amount

#### migrateModules

```solidity
function migrateModules(bytes4 _module, address _newImplementation) external;
```

Hot-swap a module implementation (e.g., Deposit/Redeem/Settlement/Fee/Migration managers) in a controlled way. For future vaults, the factory can update defaults; for this vault, use `migrateModules`.

#### queueManagementFee

```solidity
function queueManagementFee(uint8 _classId, uint32 _managementFee) external;
```

Queue a new management fee for a **class**; becomes settable after timelock via `setManagementFee()`.

#### setManagementFee

```solidity
function setManagementFee(uint8 _classId) external;
```

Apply the queued management fee after the timelock for the specified class.

**Emits:**

* `NewManagementFeeSet(uint8 classId, uint32 managementFee)`

#### queuePerformanceFee

```solidity
function queuePerformanceFee(uint8 _classId, uint32 _performanceFee) external;
```

Queue a new performance fee for a **class**.

#### setPerformanceFee

```solidity
function setPerformanceFee(uint8 _classId) external;
```

Apply the queued performance fee after the timelock for the specified class.

**Emits:**

* `NewPerformanceFeeSet(uint8 classId, uint32 performanceFee)`

#### setVaultTreasury

```solidity
function setVaultTreasury(address _vaultTreasury) external;
```

Set the vault treasury address where fees are collected. This function is called by the manager role.

**Emits:**

* `VaultTreasurySet(address vaultTreasury)`

#### collectFees

```solidity
function collectFees() external returns (
    uint256 _managementFeesToCollect,
    uint256 _performanceFeesToCollect
);
```

Collect all accumulated management and performance fees. This function is called by the ACCOUNTANT role and transfers fees to both the vault treasury and Aleph treasury based on the configured fee splits.

**Returns:**
* `_managementFeesToCollect`: Total management fees collected
* `_performanceFeesToCollect`: Total performance fees collected

**Emits:**

* `FeesCollected(uint256 managementFeesCollected, uint256 performanceFeesCollected)`

#### pause / unpause

```solidity
function pause(bytes4 _pausableFlow) external;
function unpause(bytes4 _pausableFlow) external;
```

Granularly halt/resume specific flows. Available pausable flows:
* `DEPOSIT_REQUEST_FLOW`: Pause new deposit requests
* `REDEEM_REQUEST_FLOW`: Pause new redemption requests
* `SETTLE_DEPOSIT_FLOW`: Pause deposit settlements
* `SETTLE_REDEEM_FLOW`: Pause redemption settlements
* `WITHDRAW_FLOW`: Pause withdrawal of redeemable amounts

**Emits:**

* `FlowPaused(bytes4 flow, address pauser)`
* `FlowUnpaused(bytes4 flow, address unpauser)`

#### requestDeposit

```solidity
function requestDeposit(
    RequestDepositParams calldata _requestDepositParams
) external returns (uint48 _batchId)
```

Requests a deposit of underlying assets into a specific share class. Tokens are transferred into the vault and recorded against the current batch. Only one deposit request is allowed per batch per user.

**RequestDepositParams:**
* `classId`: The share class ID to deposit into
* `amount`: The amount of underlying tokens to deposit
* Additional auth parameters if auth is enabled

Returns:

* `batchId`: The batch to which this deposit was assigned.

Emits:

* `DepositRequest(address indexed user, uint256 amount, uint48 batchId)`&#x20;

#### settleDeposit

```solidity
function settleDeposit(
    SettlementParams calldata _settlementParams
) external;
```

Settles all pending deposits for a specific class by finalizing share distribution based on the updated NAV. This function:

* Mints shares for users in the appropriate series
* Transfers deposited tokens to the custodian
* Updates total assets and shares

**SettlementParams:**
* `classId`: The share class to settle
* `batchId`: The batch ID to settle up to
* `newTotalAssets`: Updated total assets based on NAV
* Additional auth parameters if auth is enabled

This function is called by the ORACLE role.

#### requestRedeem

```solidity
function requestRedeem(
    RedeemRequestParams calldata _redeemRequestParams
) external returns (uint48 _batchId);
```

Requests a redemption from a specific share class. The request is recorded against the current batch and processed asynchronously. Underlying assets are made available after settlement.

**RedeemRequestParams:**
* `classId`: The share class ID to redeem from
* `shareAmount`: The amount of shares to redeem (as a percentage of total user assets in class, denominated in 1e18)

Returns:

* `batchId`: The batch to which this withdrawal was assigned.

Emits:

* `RedeemRequest(address indexed user, uint256 shares, uint48 batchId)`

#### settleRedeem

```solidity
function settleRedeem(
    SettlementParams calldata _settlementParams
) external;
```

Settles all pending redeem requests for a specific class using the updated NAV. This function:

* Calculates asset value based on current NAV
* Burns shares from appropriate series
* Makes assets available for withdrawal

**SettlementParams:**
* `classId`: The share class to settle
* `batchId`: The batch ID to settle up to
* `newTotalAssets`: Updated total assets based on NAV
* Additional auth parameters if auth is enabled

This function is called by the ORACLE role.

#### withdrawRedeemableAmount

```solidity
function withdrawRedeemableAmount() external;
```

Withdraws all redeemable assets that have been settled for the user. Transfers the underlying tokens from the vault to the user.

### Read Methods

#### currentBatch

```solidity
function currentBatch() external view returns (uint48);
```

Current active batch id (used to index per-batch deposit/redeem registries).

#### totalAssets

```solidity
function totalAssets() external view returns (uint256)
```

Returns the latest total value of vault holdings, based on oracle-settled NAV, for all classes / series.

#### shareClasses

```solidity
function shareClasses() external view returns (uint8);
```

Returns the number of share classes created in the vault.

#### name

```solidity
function name() external view returns (string memory);
```

Returns the vault name.

#### manager

```solidity
function manager() external view returns (address);
```

Returns vault manager address.

#### oracle

```solidity
function oracle() external view returns (address);
```

NAV oracle address.

#### operationsMultisig

```solidity
function operationsMultisig() external view returns (address);
```

Aleph Protocol multisig for ops/safety controls.

#### guardian

```solidity
function guardian() external view returns (address);
```

Guardian role address.

#### authSigner

```solidity
function authSigner() external view returns (address);
```

KYC attestation signer.

#### underlyingToken

```solidity
function underlyingToken() external view returns (address);
```

Returns the address of the underlying ERC-20 token.

#### custodian

```solidity
function custodian() external view returns (address);
```

Custodian address.

#### vaultTreasury

```solidity
function vaultTreasury() external view returns (address);
```

Returns the vault treasury address where vault's portion of fees are collected.

#### managementFee

```solidity
function managementFee(uint8 _classId) external view returns (uint32);
```

Current management fee for a share **class**.

#### performanceFee

```solidity
function performanceFee(uint8 _classId) external view returns (uint32);
```

Current performance fee for a share **class**.

#### minDepositAmount

```solidity
function minDepositAmount(uint8 _classId) external view returns (uint256);
```

Returns the minimum undelrying amount accepted as a deposit for a **class.**

#### maxDepositCap

```solidity
function maxDepositCap(uint8 _classId) external view returns (uint256);
```

Returns the maximum underlying token capacity for a **class.**

#### totalAssetsPerClass

```solidity
function totalAssetsPerClass(uint8 _classId) external view returns (uint256);
```

Returns the total assets for a given **class**.

#### totalAssetsOfClass

```solidity
function totalAssetsOfClass(uint8 _classId) external view returns (uint256[] memory);
```

Returns an array of total assets for each series in the given class.

#### totalAssetsPerSeries

```solidity
function totalAssetsPerSeries(uint8 _classId, uint8 _seriesId) external view returns (uint256);
```

Returns the total assets for a given **class and series**.

#### totalSharesPerSeries

```solidity
function totalSharesPerSeries(uint8 _classId, uint8 _seriesId) external view returns (uint256);
```

Returns the total shares for a given **class and series**.

#### pricePerShare

```solidity
function pricePerShare(uint8 _classId, uint8 _seriesId) external view returns (uint256);
```

Returns price per share (also known as NAV per share) for a given class and series.

#### highWaterMark

```solidity
function highWaterMark(uint8 _classId, uint8 _seriesId) external view returns (uint256);
```

Returns [high-water mark](../learn/vehicle-framework/fees.md#performance-fee) for a given class and series.

#### sharesOf

```solidity
function sharesOf(uint8 _classId, uint8 _seriesId, address _user) external view returns (uint256);
```

Returns the current share balance for a user and a given class / series.

#### assetsOf

```solidity
function assetsOf(uint8 _classId, uint8 _seriesId, address _user) external view returns (uint256);
```

Returns the current asset balance for a user and a given class / series.

#### totalAmountToDeposit

```solidity
function totalAmountToDeposit(uint8 _classId) external view returns (uint256);
```

Returns the **total unsettled deposit amount** (underlying tokens) requested for the given **share class**, summed across **all batches including the current batch.**

Note: if settlement is triggered for the current batch, the **amount requested in this same batch is not settled in that cycle** and will roll into the next settlement batch.

#### totalAmountToDepositAt

```solidity
function totalAmountToDepositAt(uint8 _classId, uint48 _batchId) external view returns (uint256);
```

Returns the **total unsettled deposit amount** (underlying tokens) requested for the given **share class** at a **specific batch.**

#### depositRequestOf

```solidity
function depositRequestOf(uint8 _classId, address _user) external view returns (uint256);
```

Returns the caller’s **unsettled deposit amount** (underlying tokens) for the given **share class**, summed across **all batches including the current batch.**

#### depositRequestOfAt

```solidity
function depositRequestOfAt(uint8 _classId, address _user, uint48 _batchId) external view returns (uint256);
```

Returns the caller’s **unsettled deposit amount** (underlying tokens) for the given **share class** at a **specific batch**.

#### redeemRequestOf

```solidity
function redeemRequestOf(uint8 _classId, address _user) external view returns (uint256);
```

Returns the caller’s **unsettled redemption amount** (denominated in **shares**) for the given **share class**, summed across **all batches including the current batch**.

#### redeemRequestOfAt

```solidity
function redeemRequestOf(uint8 _classId, address _user, uint48 _batchId) external view returns (uint256);
```

Returns the caller’s **unsettled redemption amount** (in **shares**) for the given **share class** at a **specific batch**.

#### usersToDepositAt

```solidity
function usersToDepositAt(uint8 _classId, uint48 _batchId) external view returns (address[] memory);
```

Returns the **list of user addresses** with a **non-zero deposit request** in the given **share class** at the specified **batch**.

#### usersToRedeemAt

```solidity
function usersToRedeemAt(uint8 _classId, uint48 _batchId) external view returns (address[] memory);
```

Returns the **list of user addresses** with a **non-zero redemption request** (in shares) in the given **share class** at the specified batch.

#### accountant

```solidity
function accountant() external view returns (address);
```

Returns the accountant contract address responsible for fee management.

#### totalAssetsPerClass

```solidity
function totalAssetsPerClass(uint8 _classId) external view returns (uint256);
```

Returns the total assets across all series for a given class.

#### forceRedeem

```solidity
function forceRedeem(address _user) external;
```

Allows the manager to force a redemption for a specific user. This is typically used for regulatory compliance or risk management.

#### withdrawExcessAssets

```solidity
function withdrawExcessAssets() external;
```

Allows the manager to withdraw any excess assets from the vault back to the custodian. Used to manage vault liquidity.

#### isDepositAuthEnabled

```solidity
function isDepositAuthEnabled() external view returns (bool);
```

Returns KYC gate status for deposits.

#### isSettlementAuthEnabled

```solidity
function isSettlementAuthEnabled() external view returns (bool);
```

Returns KYC gate status for settlements.

#### noticePeriod

```solidity
function noticePeriod(uint8 _classId) external view returns (uint48);
```

Returns the notice period (in batches) required for redemptions in the specified class.

#### lockInPeriod

```solidity
function lockInPeriod(uint8 _classId) external view returns (uint48);
```

Returns the lock-in period (in batches) for the specified class.

#### minUserBalance

```solidity
function minUserBalance(uint8 _classId) external view returns (uint256);
```

Returns the minimum balance requirement for users in the specified class.

#### minRedeemAmount

```solidity
function minRedeemAmount(uint8 _classId) external view returns (uint256);
```

Returns the minimum redemption amount for the specified class.

#### userLockInPeriod

```solidity
function userLockInPeriod(uint8 _classId, address _user) external view returns (uint48);
```

Returns the lock-in period end batch for a specific user in a class.

#### redeemableAmount

```solidity
function redeemableAmount(address _user) external view returns (uint256);
```

Returns the amount of underlying assets available for withdrawal by the user after redemption settlement.

#### totalFeeAmountToCollect

```solidity
function totalFeeAmountToCollect() external view returns (uint256);
```

Returns the total fees (management + performance) available to be collected across all classes.
