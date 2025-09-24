# Deposit

{% hint style="info" %}
When allocators deposit their tokens into the vault, they receive **shares** in return. These shares represent the user’s **proportional claim of the pooled assets**, including any yield earned over time.
{% endhint %}

### Asynchronous Deposit Process

The deposit flow is asynchronous, meaning tokens are accepted immediately, but shares are only issued (minted) after NAV settlement.

<figure><picture><source srcset="../../.gitbook/assets/Aleph-deposit.png" media="(prefers-color-scheme: dark)"><img src="../../.gitbook/assets/deposit-lifecycle-overview.png" alt=""></picture><figcaption></figcaption></figure>



***



The process consists of two phases:

### Request Deposit

Deposits are asynchronous: the request is instant, but shares are minted upon NAV settlement.

<figure><picture><source srcset="../../.gitbook/assets/Aleph-deposit-request.png" media="(prefers-color-scheme: dark)"><img src="../../.gitbook/assets/deposit-request-flow.png" alt=""></picture><figcaption></figcaption></figure>

1. Users initiate deposits by calling `requestDeposit` function with:
   * `classId`: The share class to deposit into.
   * `amount`: The amount of underlying tokens to deposit.
   * `authSignature`: (if KYC/auth is enabled).
2. The deposit amount is transferred to the vault, and requests are recorded into a batch.
3. Users can track their pending deposit requests using:
   * `depositRequestOf(classId, user)`: total pending deposits across all batches.
   * `depositRequestOfAt(classId, user, batchId)`: deposits for a specific batch.



**Important:**

* Only one deposit request is allowed per user per batch.
* Minimum deposit amounts are enforced per class.
* Maximum deposit caps can be set per class.



***

### Settle Deposit

Upon settlement, the Oracle calls `settleDeposit` to finalize all queued deposits for a specific series. The number of shares issued is calculated based on the current NAV and total share supply.

<figure><picture><source srcset="../../.gitbook/assets/Aleph-settle-deposit.png" media="(prefers-color-scheme: dark)"><img src="../../.gitbook/assets/deposit-settlement-flow.png" alt=""></picture><figcaption></figcaption></figure>

1. Manager initiates settle deposit flow using the Oracle by calling `settleDeposit` :
   * `classId`: The share class to settle.
   * `batchId`: The batch to settle up to (excluding current batch).
   * `newTotalAssets`: Updated NAV from off-chain calculations.
   * `authSignature`: (if settlement auth is enabled).
2. The vault calculates shares to mint based on:
   * Current price per share (NAV / total shares).
   * Deposited amounts for each user.
3. New shares are minted and allocated to depositors in the appropriate series:
   * Lead series (Series 1) for initial deposits.
   * New series created if performance fees apply.
4. Deposited tokens are transferred to the manager address.
5. Management and performance fees are calculated and accrued.
6. Series may be consolidated if conditions are met.
