# AlephOS

### Overview

AlephOS is the primary console for fund managers and admins of an Aleph Vehicle, leveraging an intuitive interface that translates complex on-chain and off-chain operations into actionable tasks.

The web application is designed to streamline Vehicle configuration and deployment, review and approve allocator onboarding, initiate NAV settlement processes, generate reports, and monitor operational and financial activity using a metrics cockpit.

<figure><img src="../../.gitbook/assets/alephos-architecture.png" alt="" width="563"><figcaption></figcaption></figure>

### Features

* **Manage Onboarding:** Review and approve allocators seamlessly.
* **On-demand NAV:** Live tracking powered by the NAV Engine.
* **Settlement:** Initiate NAV settlement process to finalize deposits and redemptions with T+0 processing that dynamically mints and burns LP shares.
* **Dashboard:** Key metrics of fund operational activity and financial performance, both allocator-level and fund-level.
* **Compliance:** Configure a customized onboarding process with template-based filing across jurisdictions.
* **Reporting:** Generate and export reports to meet regulatory requirements.
* **Activity Logs:** Complete audit trail of all operations, transactions, and administrative actions for regulatory compliance and transparency.

***

### Functions

#### **Deployment and Configuration**

* Set up a new Aleph Vault instance with custom parameters.
* Connect multiple trading venues to NAV Engine by uploading API keys.
* Configure key controls such as fee logic, class parameters, minimum ticket, and notice period.
* Configure KYC/AML workflows.
* Upload subscription agreement.

#### **Onboarding**

* Manage LPA/KYC compliance documents.
* Review and approve allocators onboarding.
* Review deposit and redemption requests.

#### Settlement

* Fetch iNAV and initiate NAV settlement process.
* Review and approve NAV figure.
* Settle pending deposits.
* Settle pending redemptions.
* Review and redeem performance and management fees.

#### Monitoring

* Monitor key performance metrics and operational activity.
* View allocator personal and financial information.
* View fees accumulation.
* Generate fund-level and allocator-level reports.

#### Safegrounds

* Pause/unpause critical flows.
* Set Guardian account.
* Enforce full redemption of an individual allocator
