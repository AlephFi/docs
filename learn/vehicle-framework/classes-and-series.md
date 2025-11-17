# Classes & Series

Aleph Vaults are designed to accommodate a diverse range of allocator economic terms through a two-tiered structure: **Classes and Series.**&#x20;

This design enables managers to offer distinct terms to different allocator groups while ensuring fair and accurate fee calculations based on each investor's unique entry point.

### Classes

A Share Class defines the participation terms for a group of allocators within an Aleph Vehicle. Each class operates under its own set of rules, allowing for customized fee structures and investment parameters.

#### Key Characteristics:

* Each share class holds unique economic terms.
* Classes can be configured with specific deposit requirements.
* Allows for offering different terms to various types of allocators, and can be used to create promotional or incentive-based fee arrangements for specific cohorts of investors.

### Series&#x20;

A Series is a sub-division within a share class that groups investors by their entry cohort. The primary purpose of a series is to ensure that Performance fees are calculated only on the gains an investor has actually accrued since inception. This is accomplished by maintaining a unique High-Water Mark (HWM) for each series.

#### High-Water Mark (HWM)

The High-Water Mark (HWM) is the price-per-share (PPS) threshold that a series must exceed before any performance fee can be charged. If the current PPS is less than or equal to the series' HWM, no performance fee is accrued for that series.

#### Types of Series

* Lead Series (ID 0): The foundational series for a share class. Over time, other series may be consolidated back into the lead series for operational efficiency.
* Outstanding Series (ID ≥ 1): Created to isolate new investors during periods when the lead series PPS is below its HWM. Each additional series begins with its own HWM baseline to ensure new entrants are not charged performance fees on gains they did not participate in.

#### Creation of New Series

An additional series is automatically created during the settlement process only when all of the following conditions are met:

1. The share class has a non-zero performance fee.
2. The lead series' current PPS is below its HWM.
3. There are new pending deposits to settle in the current cycle.

#### **Consolidation Logic**

When a new high watermark is set for lead series and there are outstanding series, all series are consolidated into the lead series

1. User shares are converted proportionally during consolidation to maintain fair value
2. This mechanism ensures proper performance fee calculation while maintaining operational efficiency

