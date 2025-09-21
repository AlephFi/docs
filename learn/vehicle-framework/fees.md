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

Fees are accrued during settlement process. At the end of each batch window, the Oracle provides the necessary pricing inputs to the system. During this process, the accumulated fee amounts are calculated, and new shares are minted to the designated fee recipient accounts. These shares represent the value of the fees collected.

### **Accountant**

&#x20;The Accountant is a smart contract that manages the collection and distribution of all accumulated fees. Fees are accumulated as shares and are minted to a designated virtual recipient account per individual vault.

* **Distribution:** Once accumulated, fee shares can be materialized for the underlying assets of the vault. When requested, the fee shares are burned, and the accountant transfers the amount to the vault treasury.
* **Changing Treasury:** Managers can define the address of the vault treasury in which they wish to receive the funds. They can do so by calling the `setVaultTreasury` function.
