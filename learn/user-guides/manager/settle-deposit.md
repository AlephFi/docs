# Settle Deposit

The **Settle Deposit** flow is responsible for converting all pending deposit requests into Vault Shares.

**When:** Monthly (or as scheduled by your fund)

<figure><img src="../../../.gitbook/assets/image (28).png" alt="" width="563"><figcaption></figcaption></figure>

### Before You Begin

Make sure the NAV is up to date for the Vault.

{% stepper %}
{% step %}
### Open Manager App

* Go to your **Vault Dashboard**
* Click the **“Settle Deposit”** button\
  &#xNAN;_(Only visible if you have permission and there are unsettled deposits)_
{% endstep %}

{% step %}
### Platform Fees Are Accrued

Before settling the deposits:

* Aleph Platform fees is calculated.
* Corresponding Fee Shares are minted to the Aleph Platform Fee Recipient.
{% endstep %}

{% step %}
### Deposits Are Converted Into Vault Shares

For each Allocators:

* The system calculates how many Vault Shares they should receive.
  * This is based on the current NAV and their deposited amount.
* Vault Shares are minted
{% endstep %}

{% step %}
### Funds Are Transferred to the Custodian

Once all batches are processed:

* The **entire ERC-20 token amount** collected from Allocators is transferred to the **Custodian address** (which you configured during Vault setup).
* This capital is now available to you for off-chain yield strategies.
{% endstep %}
{% endstepper %}
