# Deposit

{% hint style="info" %}
When allocators deposit their tokens into the vault, they receive **shares** in return. These shares represent the user’s **proportional claim of the pooled assets**, including any yield or interest earned over time.
{% endhint %}

### Asynchronous Deposit Process

The deposit flow is asynchronous and follows the ERC-7540 standard, meaning tokens are accepted immediately, but shares are only issued (minted) after NAV settlement.&#x20;

<figure><img src="../../.gitbook/assets/image (59).png" alt="" width="563"><figcaption></figcaption></figure>



The process consists of two phases:

### Request Deposit

Deposits are asynchronous: the deposit action is instant, but shares are minted upon NAV settlement.

<figure><img src="../../.gitbook/assets/image (4).png" alt="" width="563"><figcaption></figcaption></figure>

1. Users initiate deposits by calling `requestDeposit` function.&#x20;
2. The deposit amount is transferred to the vaul&#x74;**,** and requests are recorded into a batch.
3. Users can track their Pending deposit requests using `pendingDepositRequest` function.



### Settle Deposit

Upon settlement, the NAV Engine calls `settleDeposit` to finalize all queued deposits. The number of shares issued is calculated based on the current Net Asset Value (NAV) and total share supply.

<figure><img src="../../.gitbook/assets/image (8).png" alt="" width="563"><figcaption></figcaption></figure>

1. Manager initiates settle deposit flow, NAV Engine calls `settleDeposit`.
2. The number of shares is calculated from the final net NAV and shares supply.
3. New shares are minted and allocated to each depositor.
4. Deposited tokens are transferred to the Manager.
5. For performance-fee classes, new outstanding series may be created or consolidate&#x64;**.**&#x20;

