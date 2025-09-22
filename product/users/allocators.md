# Allocators

### Overview

Individual and institutional allocators who deposits digital assets into manager Vault. Allocators complete KYC/AML, sign an agreement, and are approved by the Manager before deposits.

For Allocator, Aleph enables instant access to institutional yield opportunities.

<figure><img src="../../.gitbook/assets/allocators-overview.png" alt=""><figcaption></figcaption></figure>

### Key Details

* Allocators must be whitelisted and complete the required [verification](allocators.md#verification) before being eligible to access the manager vault.
* Eligible allocators can queue their deposit at any given time by executing a deposit request.
* Upon [settlement](../../learn/flows/settlement.md), the deposited assets are transferred to the manager, the vault mints new shares, and the allocators receive shares proportional to the vault AUM.
* A share unit represents the principal and accrued interest, and the underlying share value is derived from the yield-generating activities of the manager.
* Allocators can queue their redemption by executing a redeem request.
* Upon settlement, the redeemed assets are transferred to the allocator, and the vault burns the specified shares.
* Allocators can view the pending deposit and redeem request through the Allocator app.

### Allocator App

The Allocator web application is a digital storefront for seamless interactions. Through the app, allocators can explore manager performance, deposit assets, redeem their proportional value of the vault, and monitor their multi-strategy portfolio.

<figure><img src="../../.gitbook/assets/allocator-process-flow.png" alt="" width="563"><figcaption></figcaption></figure>

#### Features

* **Discovery** - Browse available managers with detailed performance metrics.
* **Onboarding** - Internet-native and instant onboarding application.
* **Portfolio** - Monitor multi-manager portfolio with live financial metrics.
* **Transparency** - Auditable trail of NAV updates, settlements, and capital distribution.

### Verification

KYC/AML, liveliness check, and subscription agreement are processed through the Allocator App and approved by the manager through AlephOS. This process includes credentials and information, including jurisdiction, source of capital, source of wealth, source of funds, directors, significant controllers, and letters of authorization.
