# Redemption

{% hint style="info" %}
When allocators request a withdrawal by redeeming their shares, they receive a proportional amount of the underlying assets from the vault, reflecting their claim on the pooled funds and accumulated yield.
{% endhint %}

### Asynchronous Redeem Lifecycle

Redemptions are asynchronous and follow the ERC-7540 standard, meaning tokens are transferred after NAV settlement, ensuring fair asset distribution.&#x20;

<figure><img src="../../.gitbook/assets/image (60).png" alt="" width="563"><figcaption></figcaption></figure>



The process consists of two phases:

### Request Redeem

Allocators initiate a redemption by entering the amount of underlying ERC-20 tokens.

<figure><img src="../../.gitbook/assets/image (9).png" alt="" width="563"><figcaption></figcaption></figure>



1. The proportional amount of shares are calculated based on the last settled NAV.
2. The redeem requests are recorded in a batch managed by the Vault.
3. Users can track their Pending redeem requests using `pendingRedeemRequest`  function.



### Settle Redeem

Upon NAV settlement, the NAV Engine calls  `settleRedeem` to finalize all pending redemptions.&#x20;

<figure><img src="../../.gitbook/assets/image (10).png" alt="" width="563"><figcaption></figcaption></figure>





1. The calculated number of shares for each redeemer is burned during settlement.
2. The Vault calculates the exact amount of tokens each redeemer is entitled to receive.
3. The corresponding tokens become available for withdrawal by the redeemer.
4. Users can withdraw the tokens through the Allocator App.
