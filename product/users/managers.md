# Managers

### Overview

The actor responsible for operating a specific Aleph Vault. Managers offer yield strategies to allocators on Aleph, leveraging proprietary expertise to generate uncorrelated returns.

For Managers, Aleph introduces a new rail for capital formation markets.

<figure><img src="../../.gitbook/assets/image (30).png" alt=""><figcaption></figcaption></figure>

### Key Details

* Managers must be whitelisted before being eligible to use Aleph infrastructure.&#x20;
* Manager self-deploys its own Vault through Aleph Factory and configures bespoke onboarding processes and subscription requirements.&#x20;
* Manager reviews and approves allocators onboarding on an ongoing basis, authorizes them to access the vault contracts. Once eligible, allocators can submit a deposit request at any given time.&#x20;
* Prior to settlement, the manager must review and sign the suggested NAV update retrieved from the NAV Engine, enabling the vault to automatically settle deposit and redemption requests.&#x20;
* Upon settlement, the manager's wallet must maintain sufficient funds to cover accrued allocator fees and redemption requests.
