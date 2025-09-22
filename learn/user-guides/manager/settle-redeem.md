# Settle Redeem

The Settle Withdraw flow is responsible for fulfilling user redemption requests by returning ERC-20 tokens and burning their Vault Shares.&#x20;

**When:** Monthly (or as scheduled by your fund)

<figure><img src="../../../.gitbook/assets/manager-settle-redeem.png" alt="" width="563"><figcaption></figcaption></figure>

{% stepper %}
{% step %}
### Open Manager App

* Navigate to your **Vault Dashboard**
* Click the **“Settle Redeem”** button\
  (_Only Enabled if pending redemptions exist_)
{% endstep %}

{% step %}
### Review Settlement NAV

* Review the proposed settlement NAV&#x20;
* Approve it to proceed with redemption settlement.
{% endstep %}

{% step %}
### Ensure Token Availability

To ensure the Vault has enough ERC-20 tokens, transfer the required ERC-20 tokens from the Manager wallet back into the Vault.

{% hint style="warning" %}
If the Vault does not have enough tokens, the `settleRedeem()` transaction will revert.
{% endhint %}
{% endstep %}

{% step %}
### Settlement Process

When settlement is executed:

* The corresponding Vault Shares are burned.
* Fee Shares are minted to the Fee Recipients.
* Redeemers’ ERC-20 tokens become available for withdrawal.
{% endstep %}
{% endstepper %}

In the end The **Manager App** reflects:

* Reduced Vault share supply
* Updated asset balances
* Redemption batch records
