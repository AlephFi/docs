# Aleph Vault Factory

The **AlephVaultFactory** is the entry point for deploying new Aleph Vaults. Each Vault deployed through this factory conforms to the **ERC-7540 standard.**

### Write Methods

#### deployVault

Deploys a new Vault using the current protocol configuration stored in the factory.

```solidity
function deployVault(IAlephVault.UserInitializationParams calldata _userInitializationParams)
    external
    returns (address);
```

<table><thead><tr><th width="160.33203125">Parameter</th><th>Description</th></tr></thead><tbody><tr><td><code>name</code></td><td>Name of the Vault</td></tr><tr><td><code>configId</code></td><td>Unique vault configuration identifier</td></tr><tr><td><code>manager</code></td><td>Address that will operate the Vault</td></tr><tr><td><code>underlyingToken</code></td><td>ERC-20 token to be accepted for deposits</td></tr><tr><td><code>custodian</code></td><td>Custodian address for holding post-deposit funds</td></tr><tr><td><code>managementFee</code></td><td>Initial management fee for the default shareholder class</td></tr><tr><td><code>performanceFee</code></td><td>Initial performance fee for the default shareholder class</td></tr><tr><td><code>minDepositAmount</code></td><td>Initial minimum deposit amount for the default shareholder class</td></tr><tr><td><code>maxDepositCap</code></td><td>Initial maximum deposit cap for the default shareholder class</td></tr><tr><td><code>authSignature</code></td><td>Optional bootstrap KYC signature</td></tr></tbody></table>

Returns:

* `address`: Address of the deployed Vault
