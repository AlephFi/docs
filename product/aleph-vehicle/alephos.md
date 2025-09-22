# AlephOS

### Overview

AlephOS is the primary console for fund managers and admins of an Aleph Vehicle, leveraging an intuitive interface that translates complex on-chain and off-chain operations into actionable tasks.&#x20;

The web application is designed to streamline Vehicle configuration and deployment, review and approve allocator onboarding, initiate NAV settlement processes, generate reports, and monitor operational and financial activity using a metrics cockpit.

<figure><img src="../../.gitbook/assets/alephos-architecture.png" alt="" width="563"><figcaption></figcaption></figure>

### Features

* **Manage Onboarding:** Review and approve allocators seamlessly.&#x20;
* **On-demand NAV:** Live tracking powered by the NAV Engine.
* **Settlement:** Initiate NAV settlement process to finalize deposits and redemptions with T+0 processing that dynamically mints and burns LP shares.
* **Dashboard:** Key metrics of fund operational activity and financial performance, both allocator-level and fund-level.&#x20;
* **Compliance:** Configure a customized onboarding process with template-based filing across jurisdictions.
* **Reporting:** Generate and export reports to meet regulatory requirements.&#x20;
* **Activity Logs:** Complete audit trail of all operations, transactions, and administrative actions for regulatory compliance and transparency.

***

### Functions

#### **Deployment and Configuration**&#x20;

* Set up a new Aleph Vault instance with custom parameters.
* Connect multiple trading venues to NAV Engine by uploading API keys.
* Configure key controls such as fee logic, class parameters, minimum ticket, and notice period.&#x20;
* Configure KYC/AML workflows.&#x20;
* Upload subscription agreement.&#x20;

#### **Onboarding**

* Manage LPA/KYC compliance documents.
* Review and approve allocators onboarding.
* Review deposit and redemption requests.&#x20;
* Generate fund-level and allocator-level reports.

#### Settlement&#x20;

* Fetch iNAV and initiate NAV settlement process.
* Review and approve NAV figure.
* Settle pending deposits.
* Settle pending redemptions.
* Review and redeem performance and management fees.

#### Monitoring&#x20;

* Monitor key performance metrics and operational activity.&#x20;
* View allocator personal and financial information.&#x20;
* View fees accumulation.&#x20;

#### Safegrounds

* Pause/unpause critical flows.
* Set Guardian account.&#x20;
* Enforce full redemption of an individual allocator&#x20;



