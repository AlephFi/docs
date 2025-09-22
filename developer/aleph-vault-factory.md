# Aleph Vault Factory

The **AlephVaultFactory** is the entry point for deploying new Aleph Vaults. Each Vault deployed through this factory implements Aleph's custom asynchronous vault architecture.

### Write Methods

#### deployVault

Deploys a new Vault using the current protocol configuration stored in the factory.

```solidity
function deployVault(IAlephVault.UserInitializationParams calldata _userInitializationParams)
    external
    returns (address);
```

<table><thead><tr><th width="160.33203125">Parameter</th><th>Description</th></tr></thead><tbody><tr><td><code>name</code></td><td>Name of the Vault</td></tr><tr><td><code>configId</code></td><td>Unique vault configuration identifier</td></tr><tr><td><code>manager</code></td><td>Address that will operate the Vault</td></tr><tr><td><code>underlyingToken</code></td><td>ERC-20 token to be accepted for deposits</td></tr><tr><td><code>custodian</code></td><td>Custodian address for holding post-deposit funds</td></tr><tr><td><code>vaultTreasury</code></td><td>Treasury address where vault's portion of fees are sent</td></tr><tr><td><code>shareClassParams</code></td><td>Parameters for the default share class including:</td></tr><tr><td>├ <code>managementFee</code></td><td>Management fee rate in basis points</td></tr><tr><td>├ <code>performanceFee</code></td><td>Performance fee rate in basis points</td></tr><tr><td>├ <code>noticePeriod</code></td><td>Notice period in batches for redemptions</td></tr><tr><td>├ <code>lockInPeriod</code></td><td>Lock-in period in batches for new investors</td></tr><tr><td>├ <code>minDepositAmount</code></td><td>Minimum deposit amount</td></tr><tr><td>├ <code>minUserBalance</code></td><td>Minimum user balance requirement</td></tr><tr><td>├ <code>maxDepositCap</code></td><td>Maximum deposit cap for the class</td></tr><tr><td>└ <code>minRedeemAmount</code></td><td>Minimum redemption amount</td></tr><tr><td><code>authSignature</code></td><td>Optional bootstrap KYC signature for vault creation</td></tr></tbody></table>

Returns:

* `address`: Address of the deployed Vault
