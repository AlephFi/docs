# Redemption

{% hint style="info" %}
When allocators request a withdrawal by redeeming their shares, they receive a proportional amount of the underlying assets from the vault, reflecting their claim on the pooled funds and accumulated yield.
{% endhint %}

### Asynchronous Redeem Lifecycle

Redemptions are asynchronous, meaning tokens are transferred after NAV settlement, ensuring fair asset distribution.

<figure><img src="../../.gitbook/assets/redemption-lifecycle-overview.png" alt="" width="563"><figcaption></figcaption></figure>



***



The process consists of multiple phases:

### Request Redeem

Allocators initiate a redemption by specifying the portion of their holdings to redeem.

<figure><img src="../../.gitbook/assets/redemption-request-flow.png" alt="" width="563"><figcaption></figcaption></figure>

1. Users call `requestRedeem` :
   * `classId`: the share class to redeem from.
   * `estAmountToRedeem`: the estimated amount of underlying tokens to redeem.
2. The system calculates share units based on the current PPS at the time of request.
3. The redemption request is recorded in the current batch.
4. Users can track their pending redemption requests using:
   * `redeemRequestOf(classId, user)`: Total pending redemptions across all batches
   * `redeemRequestOfAt(classId, user, batchId)`: Redemptions for a specific batch



**Important Notes:**

* Notice period: Users may need to wait a certain number of batches before redemption.
* Lock-in period: New investors may be locked for a specified number of batches.
* Minimum redemption amounts may apply per class.
* The actual amount received after settlement may differ from the requested amount due to NAV changes and fees.



***

### Settle Redeem

Upon NAV settlement, the Oracle calls `settleRedeem` to finalize all pending redemptions.

<figure><img src="../../.gitbook/assets/redemption-settlement-flow.png" alt="" width="563"><figcaption></figcaption></figure>

1. Oracle calls `settleRedeem` :
   * `classId`: The share class to settle
   * `batchId`: The batch to settle up to (excluding current batch)
   * `newTotalAssets`: Updated NAV from off-chain calculations
   * `authSignature`: (if settlement auth is enabled)
2. For each redeemer:
   * Shares are proportionally deducted from all series they hold.
   * The exact amount of underlying tokens is calculated based on current NAV.
   * Shares are burned from the appropriate series.
3. Redeemed assets are added to the user's `redeemableAmount` balance.
4. Management and performance fees are calculated and accrued.

### Withdraw Redeemable Amount

After settlement, users can withdraw their redeemable assets:

1. Users call `withdrawRedeemableAmount()` to claim all settled redemptions.
2. The vault transfers the underlying tokens from vault to the user.
3. The user's `redeemableAmount` balance is reset to zero.
