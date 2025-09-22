---
hidden: true
---

# Glossary

#### Yield Bearing Vaults

Yield-bearing vaults (also called tokenized vaults) are smart contracts that manage user deposits in a way that generates yield over time, typically by allocating assets to external strategies (on-chain or off-chain). These strategies can include ??

#### Asset

The underlying token (typically an ERC-20 token) managed by the Vault . It has units defined by the corresponding EIP-20 contract.&#x20;

#### Shares

When users deposit their assets into the vault, they receive vault-specific tokens called _Shares,_ representing their proportional ownership of the vault’s underlying assets and yield.

#### **Redeeming**

Redeeming is the process of converting your shares back into the original underlying asset, allowing you to receive part or all of your original deposit - plus any yield or interest earned over time.

So for example, If you deposited **100 USDC** into a vault and received **100 shares**, and over time the vault's value grew by 30%, those shares would now be worth **130 USDC**. You could call `redeem(100)` to burn your 100 shares and receive **130 USDC** back.

#### [**ERC-4626**](https://eips.ethereum.org/EIPS/eip-4626)

The base standard for yield-bearing vaults, defining methods for user deposits, withdrawals, asset-to-share conversions, and vault balance tracking. It extends the functionality of the ERC-20 token standard.

#### **Asynchronous Vaults**

Vault systems designed for asynchronous deposit and redemption flows, suitable for real-world asset protocols, off-chain strategies, and other use cases where immediate execution is not possible or optimal.

#### **Asynchronous Flow**

A process where user actions (like deposit or redeem) are **not** executed immediately, but queued and settled later in batches.

#### Net Asset Value (NAV)

The total value of assets held by the vault at a given time, used to calculate the value of each share. Its updated during batch settlement.

#### Custodian

A secure smart contract responsible for holding the vault's deposited tokens and forwarding them to off-chain or external strategies for yield generation.

#### **Oracle**

A trusted role that updates the vault NAV for batch settlement by calling `settleDeposit()` or `settleRedeem()` with updated asset values from off-chain calculations.

#### **Share Class**

A distinct category within a vault that has its own fee structure, investment limits, and redemption terms. Each class can have different management fees, performance fees, lock-in periods, and notice periods.

#### **Share Series**

Sub-divisions within a share class created for tracking performance fees and high-water marks. New series are created when new deposits occur after performance fee events.

#### **High-Water Mark (HWM)**

The highest price-per-share a series has achieved, used to ensure performance fees are only charged on new gains above previous peaks.

#### **Accountant**

A protocol-level smart contract that manages fee collection and distribution, splitting fees between vault treasuries and the Aleph protocol treasury.

#### **Batch Duration**

The fixed time period for each settlement batch (e.g., daily, weekly). Deposits and redemptions are processed at the end of each batch period.

#### **Lock-in Period**

The number of batches a new investor must wait before being allowed to redeem their shares, protecting the fund from short-term speculation.

#### **Notice Period**

The number of batches in advance that an investor must request a redemption before it can be processed, allowing managers to prepare for withdrawals.

