# Fees

The fee structure for an Aleph Vehicle is designed to be configurable by managers, with two primary fee types: management fee and performance fee.&#x20;

These fees accrue and are processed during the batch settlement cycles, ensuring transparency and accuracy.

### **Management Fee**

The Management Fee is a time-based fee calculated on the assets under management (AUM). It is expressed in basis points per year and accrues on a pro-rata basis over time.

**Calculation**

The fee is calculated and charged based on the duration elapsed since the last fee payment, as a fraction of the total year. This ensures that the fee is fair and reflects the time the assets were managed, regardless of the batch frequency.

```
Management Fee = AUM * ( managementFeeBps / 10,000) * (elapsedTime / ONE_YEAR)
```

### **Performance Fee**

The Performance Fee is a fee charged on new profits earned above a series' High-Water Mark (HWM). It is expressed in basis points (bps) and is only triggered when a series' performance exceeds its HWM.

**High-Water Mark (HWM)**

The HWM is the price-per-share (PPS) a series must exceed before any performance fee is applied. This prevents managers from charging a performance fee on gains that an investor has already paid for or that occurred before their investment.

**Calculation**

The fee is calculated on the value of gains that exceed the HWM for a specific series.

```
Price Per Share = AUM / Total Shares;

if (Price Per Share > HWM) {
  performance fee = (Price Per Share - HWM) * Total Shares * (performanceFeeBps / 10,000);
  // update HWM
}
```

### **Fee Accrual**&#x20;

Fees are accrued during the settlement process. At the end of each batch window, the Oracle provides the necessary pricing inputs to the system. During this process:

1. Management fees are calculated based on time elapsed and AUM
2. Performance fees are calculated based on gains above the High-Water Mark
3. Fee amounts are allocated to special virtual fee recipient addresses:
   - `MANAGEMENT_FEE_RECIPIENT`: Accumulates management fees
   - `PERFORMANCE_FEE_RECIPIENT`: Accumulates performance fees
4. These fees remain as shares until collected

### **Accountant**

The Accountant is a smart contract that manages the collection and distribution of all accumulated fees across the Aleph protocol. It handles fee splits between vault managers and the Aleph protocol.

#### **Fee Collection Process**

1. **Accumulation:** Fees accumulate as shares allocated to virtual recipient addresses within each vault
2. **Collection:** The manager triggers fee collection by calling `collectFees()` on the vault
3. **Transfer:** The vault transfers accumulated fee shares to the Accountant contract
4. **Distribution:** The Accountant splits fees between:
   - **Vault Treasury:** Receives the vault's portion of fees (manager's share)
   - **Aleph Treasury:** Receives the protocol's portion based on configured fee cuts

#### **Fee Splits**

The Accountant maintains configurable fee cuts for each vault:
- `managementFeeCut`: Percentage of management fees that goes to Aleph protocol (in basis points)
- `performanceFeeCut`: Percentage of performance fees that goes to Aleph protocol (in basis points)

These cuts are set by the operations multisig and determine how fees are split between the vault manager and Aleph protocol.

#### **Treasury Management**

* **Vault Treasury:** Each vault has its own treasury address where the manager's portion of fees is sent
* **Aleph Treasury:** A global treasury address for all protocol fees
* **Changing Treasury:** Managers can update their vault treasury by calling `setVaultTreasury()`

#### **Fee Limits**

To protect investors, the protocol enforces maximum fee limits:
- Maximum management fee: Defined by `MAXIMUM_MANAGEMENT_FEE` constant
- Maximum performance fee: Defined by `MAXIMUM_PERFORMANCE_FEE` constant
