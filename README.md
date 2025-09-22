---
cover: .gitbook/assets/aleph-cover-banner.png
coverY: 0
layout:
  width: default
  cover:
    visible: true
    size: hero
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
  metadata:
    visible: true
---

# Aleph Protocol Documentation

## Overview

Aleph is a financial infrastructure for uncorrelated yield strategies, connecting digital asset allocators with institutional money managers.

We build a secure, transparent, and efficient marketplace that enables allocators to instantly access money managers with diversified alpha strategies, while maintaining the key controls expected in traditional finance.

## 🏗️ Architecture

Aleph's blockchain-based architecture streamlines complex on-chain and off-chain tasks, eliminates bureaucratic frictions, reduces operational costs, and enhances capital efficiency.

### Core Components

- **Aleph Vault**: Custom smart contracts for asynchronous deposits and redemptions
- **NAV Engine**: Off-chain service for Net Asset Value calculations and settlements
- **Accountant**: Fee management and distribution system
- **AlephOS**: Operational dashboard for managers and allocators

## 👥 Who is Aleph for?

### Managers
Institutional money managers who generate uncorrelated yield strategies from economic activities outside the closed loop of on-chain protocols. The trades are executed through exchanges or prime brokerage, where the key profit drivers are:
- Liquidity depth
- Latency optimization
- Market structure
- Proprietary alpha-generating models

### Allocators
- Crypto natives
- High-net-worth individuals
- Curators
- Foundations
- Fund of funds
- Family offices
- Wealth funds
- Fintechs
- Private placement funds

They pursue over-performing, risk-adjusted returns through diversified strategies.

## 📚 Documentation Structure

### Learn
- **[Vehicle Framework](./learn/vehicle-framework/)** - Understanding the Aleph vehicle structure
- **[Flows](./learn/flows/)** - Deposit, redemption, and settlement processes
- **[User Guides](./learn/user-guides/)** - Step-by-step guides for managers and allocators

### Developer
- **[Aleph Vault](./developer/aleph-vault.md)** - Smart contract technical documentation
- **[Aleph Vault Factory](./developer/aleph-vault-factory.md)** - Vault deployment interface
- **[Addresses](./developer/addresses.md)** - Deployed contract addresses

### Product
- **[Aleph Vehicle](./product/aleph-vehicle/)** - Product architecture and components
- **[Users](./product/users/)** - User roles and capabilities

### Resources
- **[Glossary](./resources/glossary.md)** - Key terms and definitions
- **[Audits](./resources/audits.md)** - Security audit reports
- **[Legal](./resources/legal/)** - Terms of service and legal documentation

## 🔑 Key Features

### For Managers
- **Customizable Share Classes** - Multiple fee structures and investment terms
- **Automated Settlement** - Oracle-driven NAV updates and batch processing
- **Fee Management** - Flexible management and performance fee configurations
- **Access Controls** - Role-based permissions and emergency pause capabilities

### For Allocators
- **Asynchronous Operations** - Custom batch-based system for efficient capital deployment
- **Transparent NAV** - Regular pricing updates and settlement cycles
- **KYC/AML Integration** - Optional authentication for regulatory compliance
- **Multi-Series Tracking** - Performance-based share series with high-water marks

## 🛡️ Security

- Multi-role access control system
- Timelock protection for critical parameters
- Guardian monitoring for malicious activity
- Emergency pause mechanisms
- Comprehensive audit coverage

## 🚀 Getting Started

1. **For Allocators**: Start with the [Allocator Onboarding Guide](./learn/user-guides/allocator/onboarding.md)
2. **For Managers**: Begin with [Creating a Vault](./learn/user-guides/manager/create-vault.md)
3. **For Developers**: Review the [Aleph Vault Technical Documentation](./developer/aleph-vault.md)

## 📖 Additional Resources

- **Website**: [aleph.finance](https://aleph.finance)
- **Terms of Service**: [aleph.finance/terms-of-service](https://aleph.finance/terms-of-service)
- **GitHub**: [github.com/AlephFi](https://github.com/AlephFi)

---

*This documentation is continuously updated to reflect the latest protocol improvements and features.*