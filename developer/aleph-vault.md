# Aleph Vault

The **Aleph Vault** is an enhanced [ERC-7540](https://eips.ethereum.org/EIPS/eip-7540) compatible smart contract that enables users to participate in off-chain strategies by asynchronously depositing and redeeming on-chain ERC-20 assets. It uses **batch-based settlements**, where an oracle periodically finalizes deposits and redemptions based on updated Net Asset Value (NAV).

#### Vault Configuration

Each Aleph Vault is initialized with the following key parameters:

* `name` : The name of the Vault.
* `underlyingToken`: The ERC-20 token the vault accepts, also called the **underlying asset**.
* `configId`: Vault configuration identifier.
* `custodian`: The address of the **Custodian contract**, where all deposited funds are securely held and further invested off-chain or in external strategies.
* `manager`: Operations role for day-to-day vault management.
* `authSigner` : Off-chain KYC attester used when auth is enabled.
* `batchDuration`: The fixed duration of each settlement batch (e.g., hourly, daily).

### Write Methods

#### setIsAuthEnabled

```solidity
function setIsAuthEnabled(bool _isAuthEnabled) external;
```

Toggle KYC gating. When enabled, off-chain attestations from `authSigner` are enforced by policy.

**Emits:**

* `IsAuthEnabledSet(bool isAuthEnabled)`

#### createShareClass

```solidity
function createShareClass(
    uint32 _managementFee,
    uint32 _performanceFee,
    uint256 _minDepositAmount,
    uint256 _maxDepositCap
) external returns (uint8 _classId);
```

Create a new **share class** with its own fee schedule and deposit limits. Returns the new `classId`.

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
function setManagementFee() external;
```

Apply the queued management fee after the timelock.

**Emits:**

* `NewManagementFeeSet(uint8 classId, uint32 managementFee)`

#### queuePerformanceFee

```solidity
function queuePerformanceFee(uint8 _classId, uint32 _performanceFee) external;
```

Queue a new performance fee for a **class**.

#### setPerformanceFee

```solidity
function setPerformanceFee() external;
```

Apply the queued performance fee after the timelock.

**Emits:**

* `NewPerformanceFeeSet(uint8 classId, uint32 performanceFee)`

#### queueFeeRecipient

```solidity
function queueFeeRecipient(address _feeRecipient) external;
```

Queue a new `feeRecipient`.

#### setFeeRecipient

```solidity
function setFeeRecipient() external;
```

Apply the queued feeRecipient after the timelock.

**Emits:**

* `NewFeeRecipientSet(address feeRecipient)`

#### collectFees

```solidity
function collectFees() external;
```

Mint/transfer all accumulated fees to `feeRecipient`.

**Emits:**

* `FeesCollected(uint256 managementFeesCollected, uint256 performanceFeesCollected)`

#### pause / unpause

```solidity
function pause(bytes4 _pausableFlow) external;
function unpause(bytes4 _pausableFlow) external;
```

Granularly halt/resume specific flows (e.g., deposits only).

**Emits:**

* `FlowPaused(bytes4 flow, address pauser)`
* `FlowUnpaused(bytes4 flow, address unpauser)`

#### requestDeposit

```solidity
function requestDeposit(uint256 _amount) external returns (uint48 _batchId)
```

Requests a deposit of the specified `_amount` of the vault's underlying asset. Tokens are transferred into the vault and recorded against the current batch. Only one deposit request is allowed per batch per user.

Returns:

* `batchId`: The batch to which this deposit was assigned.

Emits:

* `DepositRequest(address indexed user, uint256 amount, uint48 batchId)`&#x20;

#### settleDeposit

```solidity
function settleDeposit(uint256 _newTotalAssets) external;
```

Settles all pending deposits by finalizing share distribution based on the updated `newTotalAssets`. This function:

* Mints shares for users.
* Transfers deposited tokens to the custodian.

This function is typically called by a **trusted oracle.**

#### requestRedeem

```solidity
function requestRedeem(uint256 _shares) external returns (uint48 _batchId);
```

Requests a redemption of the specified `_shares` of the user. Shares are deducted from the user's balance and recorded against the current batch. Underlying ERC20 assets are transferred to the user after the batch is settled.

Returns:

* `batchId`: The batch to which this withdrawal was assigned.

Emits:

* `RedeemRequest(address indexed user, uint256 shares, uint48 batchId)`

#### settleRedeem

```solidity
function settleRedeem(uint256 _newTotalAssets) external;
```

Settles all pending redeem requests from the last settled batch to the current one using the updated `newTotalAssets` . This function:

* Calculates asset value of shares burned
* Transfers tokens to users

This function is typically called by a **trusted oracle.**

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

#### totalShares

```solidity
function totalShares() external view returns (uint256);
```

Returns the total shares outstanding in the vault.

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

#### feeRecipient

```solidity
function feeRecipient() external view returns (address);
```

Current fee recipient.

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

#### totalSharesPerClass

```solidity
function totalSharesPerClass(uint8 _classId) external view returns (uint256);
```

Returns the total shares for a given **class**.

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

#### isAuthEnabled

```solidity
function isAuthEnabled() external view returns (bool);
```

Returns KYC gate status.
