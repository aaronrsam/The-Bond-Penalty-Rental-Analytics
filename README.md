# The Bond Penalty: Same System, Different Outcome


---

## Project Overview

This project investigates postcode-level inequality in NSW's rental bond system. Using bond lodgement and refund records from NSW Fair Trading, combined with ABS SEIFA socioeconomic disadvantage data, we found that renters in the most disadvantaged NSW postcodes lose an average of **37% of their rental bond**, compared to just **13% in the wealthiest postcodes** — a nearly 3× gap under the same legal system.

We call this structural disparity **The Bond Penalty**.

---

## Problem Statement

When a NSW tenancy ends, landlords can claim some or all of the rental bond to cover unpaid rent, cleaning, or damage. The system is governed by the same Residential Tenancies Act for every renter in NSW. Yet our analysis of 310,428 refund records shows that renters in socioeconomically disadvantaged postcodes consistently lose a far larger share of their bond than renters in affluent postcodes. This gap follows a perfectly monotonic gradient across all 10 SEIFA disadvantage deciles — there is no noise and no exception.

---

## Stakeholder Hat

**NSW Minister for Better Regulation and Fair Trading**

This project is framed as a data-driven briefing to the Minister, whose portfolio includes NSW Fair Trading and oversight of the Residential Tenancies Act.

---

## Dashboard

**Title:** The Bond Penalty: Same System, Different Outcome

**Link:** [Tableau Public — The Bond Penalty](https://public.tableau.com/app/profile/md.abtab.karim.kabbo/viz/TheRentersTaxNSW/Dashboard)

The dashboard is designed as a **decision-support tool**, not just a reporting dashboard. It allows a policymaker to:

- See the bond-loss gap between disadvantaged and affluent postcodes at a glance
- Identify specific squeezed suburbs by name and postcode
- Understand the geographic distribution of rental burden across NSW
- See which essential workers are priced out of affordable postcodes
- Understand why renters in squeezed suburbs cannot simply move elsewhere

---

## Data Sources

| Dataset | Source | Description | Year |
|---|---|---|---|
| Rental bond lodgements | NSW Fair Trading | 303,111 residential bond lodgements in NSW | 2025 |
| Rental bond refunds | NSW Fair Trading | 310,428 bond refund records including amounts paid to agent and tenant | 2025 |
| Bonds held | NSW Fair Trading | Count of active bonds held by postcode | 2025 |
| SEIFA disadvantage deciles | Australian Bureau of Statistics (ABS) | Socio-Economic Indexes for Areas — disadvantage decile and score by postcode | 2021 |

---

## Data Processing Summary

All data was processed in Python using `pandas`. Key steps:

1. **Loaded** four CSV files: `lodgements_cleaned.csv`, `refunds_cleaned.csv`, `bonds_held_cleaned.csv`, `seifa_cleaned.csv`
2. **Joined** refunds to SEIFA data on `postcode` to attach a disadvantage decile to each refund record
3. **Calculated** bond loss percentage per record: `payment_to_agent / (payment_to_agent + payment_to_tenant)`
4. **Aggregated** by SEIFA decile and postcode to produce average bond loss rates
5. **Filtered** to NSW postcodes to exclude interstate records
6. **Identified** zero-choice postcodes as those with both SEIFA decile ≤ 3 and median weekly rent at or above the NSW median of $690

The cleaned CSVs were imported directly into Tableau. No further transformation was applied inside Tableau beyond calculated fields.

---

## Tableau Dashboard Structure

| Sheet name | Chart type | Purpose |
|---|---|---|
| `01_KPI_BondPenalty` | KPI tiles | Summary statistics: bonds analysed, median rent, zero-choice suburbs, bond lost in poorest areas, median tenancy length |
| `02_Bar_BondLossByDecile` | Line / bar chart | Wealth gap — bond loss % by SEIFA disadvantage decile (1–10) |
| `03_Map_GeographyOfCrisis` | Filled map | Geography of crisis — postcode-level bond loss across NSW |
| `04_Bar_SqueezedSuburbs` | Horizontal bar | Critical list — NSW's most expensive areas by median rent |
| `05_Scatter_NoEscape` | Scatter plot | No escape — high rents in low income areas across all postcodes |
| `06_Line_EssentialWorkers` | Line chart | Locked out — essential worker affordability curve |
| `07_DataDictionary` | Text sheet | Data dictionary (technical documentation) |
| `08_Credits` | Text sheet | Credits and data source acknowledgements |

---

## Key Findings

- Renters in SEIFA Decile 1 (most disadvantaged) lose an average of **37%** of their bond
- Renters in SEIFA Decile 10 (most affluent) lose an average of **13%** of their bond
- The gap is **nearly 3×** — under the same legal system, same forms, same Fair Trading process
- The gradient is perfectly monotonic — every decile step, bond loss falls with no exceptions
- **23 postcodes** are classified as zero-choice: disadvantaged on SEIFA AND carrying rents at or above the NSW median of $690/week
- The median NSW tenancy lasts **564 days (~18.8 months)** — meaning the bond penalty repeats every time a renter moves
- **30% of tenancies end within a year** — many renters face this loss annually
- Only **11% of tenancies reach 5 years** — long-term stability is the exception, not the norm
- A Level 1 nurse on $480/week can afford only **22%** of NSW postcodes
- A Year 3 teacher on $612/week can afford only **44%** of NSW postcodes
- A police constable on $770/week can afford only **64%** of NSW postcodes

---

## Three Policy Recommendations

| Timeline | Action | Mechanism |
|---|---|---|
| NOW — 0–3 months | Mandatory itemised bond deductions | Amend Residential Tenancies Regulation 2019, Schedule 1 — Order-in-Council, no new legislation required |
| THIS YEAR — 3–12 months | Quarterly public reporting of bond outcomes by postcode | Departmental reporting directive — data already collected by Fair Trading |
| LEGACY — 12–18 months | Tenant-favour presumption in NCAT for SEIFA deciles 1–3 | NCAT procedural direction + RTA s.187 amendment — pilot in the 8 named squeezed postcodes first |

**Target outcomes from the deck:**
- 100% of bond claims itemised (NOW)
- Bond-loss gap below 2× by Q4 (THIS YEAR)
- Decile-1 bond loss reduced from 37% to 20% by 2028 (LEGACY)

---

## Quantified Impact

| Metric | Value | Type |
|---|---|---|
| Bond recoverable per year | $15.6M | Modelled |
| Bond lodgements in deciles 1–3 | 65,140 | Observed |
| Targeted postcodes | 194 | Observed |
| Residents at stake | 2.1M | Observed |

---

## Technical Features

- **Python 3 / pandas** — data cleaning, joining, aggregation, and calculated field preparation
- **Tableau Public** — dashboard with 6 coordinated visualisations plus documentation sheets
- **SEIFA join** — ABS SEIFA 2021 postcode-level data joined to NSW Fair Trading refund records on `postcode`
- **Key calculated field** — `Bond Loss %` = `SUM([payment_to_agent]) / (SUM([payment_to_agent]) + SUM([payment_to_tenant]))`

---

## Limitations

- Bond refund records do not include the **stated reason** for deductions (damage, unpaid rent, cleaning). We observe outcomes only, not justifications.
- SEIFA 2021 data may not reflect socioeconomic changes since 2021.
- The SEIFA–bond loss relationship is **correlational**, not causal. Other factors such as property age, dwelling type, or landlord behaviour may contribute.
- Postcode-level SEIFA matching means individual properties within a postcode may have different characteristics to the postcode average.
- Weekly rent figures in `lodgements_cleaned.csv` contain some non-numeric entries that were excluded from median rent calculations.

---

## How to Reproduce

1. Clone this repository: `git clone https://github.com/harshjavia18/DVN_Group11.git`
2. Run the Python cleaning scripts on the raw source data
3. Import the four cleaned CSVs into Tableau Desktop or Tableau Public
4. Join `refunds_cleaned` to `seifa_cleaned` on `postcode`
5. Create the calculated field `Bond Loss %` as documented in the Data Dictionary below
6. Rebuild sheets using the naming convention in this README

---

## Data Dictionary

Variables marked **[CALCULATED]** are derived fields and do not exist as raw columns in the source CSVs.

### `lodgements_cleaned.csv` — 303,111 rows

| Variable | Definition | Data type | Source | Notes |
|---|---|---|---|---|
| `lodgement_date` | Date the rental bond was lodged with NSW Fair Trading | String (YYYY-MM-DD) | NSW Fair Trading | Covers full year 2025. Used for temporal filtering. |
| `postcode` | NSW postcode of the rental property | Integer | NSW Fair Trading | Range 2000–2898. Join key to SEIFA data. 602 unique postcodes. |
| `dwelling_type` | Type of dwelling (coded) | String | NSW Fair Trading | Codes: F = flat, H = house, U = unit, T = townhouse, and others. Used as a filter. |
| `bedrooms` | Number of bedrooms in the dwelling | String | NSW Fair Trading | Stored as text due to non-numeric entries. Used as a filter. |
| `weekly_rent` | Weekly rent in AUD at time of lodgement | String | NSW Fair Trading | Cast to numeric for median rent calculations. NSW median = $690/week. |

### `refunds_cleaned.csv` — 310,428 rows

| Variable | Definition | Data type | Source | Notes |
|---|---|---|---|---|
| `payment_date` | Date the bond refund was processed | String (YYYY-MM-DD) | NSW Fair Trading | 251 unique dates. Used for temporal filtering. |
| `postcode` | NSW postcode of the rental property | Integer | NSW Fair Trading | 601 unique postcodes. Primary join key to SEIFA data. |
| `dwelling_type` | Type of dwelling (coded) | String | NSW Fair Trading | 11 unique codes. Consistent with lodgements table. |
| `bedrooms` | Number of bedrooms in the dwelling | String | NSW Fair Trading | Used as a filter dimension. |
| `payment_to_agent` | Bond amount kept by the landlord or agent in AUD | Integer | NSW Fair Trading | Mean: $505. Range: $0–$60,795. Zero means full bond was returned to the tenant. Numerator in the bond loss calculation. |
| `payment_to_tenant` | Bond amount returned to the tenant in AUD | Integer | NSW Fair Trading | Mean: $2,094. Range: $0–$80,000. Zero means full bond was retained by the landlord. |
| `days_bond_held` | Number of days the bond was held — proxy for tenancy length | Integer | NSW Fair Trading | Median: 564 days (~18.8 months). Mean: 918 days. Used for Median Tenancy Length KPI. |
| `bond_loss_pct` **[CALCULATED]** | Percentage of total bond kept by the landlord or agent | Float (0.0–1.0) | Derived | Formula: `payment_to_agent / (payment_to_agent + payment_to_tenant)`. Central metric of the dashboard. In Tableau: `SUM([payment_to_agent]) / (SUM([payment_to_agent]) + SUM([payment_to_tenant]))`. |
| `total_bond` **[CALCULATED]** | Total bond amount for a refund record in AUD | Integer | Derived | Formula: `payment_to_agent + payment_to_tenant`. Used to exclude $0 records if needed. |

### `bonds_held_cleaned.csv` — 649 rows

| Variable | Definition | Data type | Source | Notes |
|---|---|---|---|---|
| `postcode` | NSW postcode | Integer | NSW Fair Trading | 649 postcodes with at least one active bond. |
| `bonds_held` | Number of bonds currently held by Fair Trading for that postcode | Integer | NSW Fair Trading | Total across all postcodes: 980,458. |

### `seifa_cleaned.csv` — 2,628 rows

| Variable | Definition | Data type | Source | Notes |
|---|---|---|---|---|
| `postcode` | Postcode (national coverage) | String | ABS SEIFA 2021 | Stored as text. Cast to integer for joining to Fair Trading data. |
| `seifa_disadvantage_score` | Raw SEIFA Index of Relative Socioeconomic Disadvantage score | Float | ABS SEIFA 2021 | Range: 492–1,166. Lower = more disadvantaged. Used on x-axis of scatterplot. |
| `seifa_disadvantage_decile` | Decile rank by disadvantage (1 = most disadvantaged, 10 = most affluent) | Float | ABS SEIFA 2021 | Range: 1–10. Primary dimension in the wealth gap chart and all SEIFA-based groupings. |
| `seifa_advantage_disadvantage_score` | ABS SEIFA Index of Economic Resources score | Float | ABS SEIFA 2021 | Not used as a primary dimension in the current dashboard. |
| `seifa_advantage_disadvantage_decile` | Decile rank by economic resources | Float | ABS SEIFA 2021 | Not used as a primary dimension in the current dashboard. |
| `usual_resident_population` | Estimated usual resident population of the postcode | Float | ABS SEIFA 2021 | Used to estimate total residents affected across squeezed postcodes. |

### Additional derived fields used in Tableau

| Field | Definition | Type | Notes |
|---|---|---|---|
| `avg_days` | Average tenancy duration in days | Decimal | Derived from `days_bond_held` |
| `avg_rent` | Average weekly rent in AUD | Decimal | Derived from `weekly_rent` |
| `avg_tenant_payment` | Average bond payment returned to tenant | Decimal | Derived from `payment_to_tenant` |
| `est_landlord_bond_payment` | Estimated annual bond cost to landlord | Decimal | Derived |
| `lat` / `lon` | Postcode centroid coordinates | Decimal | OpenStreetMap / ABS |
| `lga` | Local Government Area name | String | NSW Fair Trading |
| `med_days` | Median tenancy duration in days | Decimal | Derived from `days_bond_held` |
| `median_rent` | Median weekly rent in AUD | Decimal | Derived from `weekly_rent` |
| `median_tenancy_months` | Median tenancy in months | Decimal | `med_days` divided by 30 |
| `nurse_affordable` | Affordable / Unaffordable at $480/wk nurse salary | String | Derived |
| `police_affordable` | Affordable / Unaffordable at $770/wk police salary | String | Derived |
| `population` | Estimated resident population | Integer | ABS |
| `pressure_rank` | Postcode rank by rent pressure (1 = highest) | Integer | Derived |
| `refund_ratio` | Refunds divided by lodgements | Decimal | Derived |
| `rent_band` | Rent category: Under $450 / $450–$619 / $620–$799 / $800+ | String | Derived |
| `rent_pressure` | Composite rent stress index score | Decimal | Derived |
| `seifa_adv_decile` | SEIFA Index of Advantage decile | Integer | ABS 2021 |
| `seifa_decile` | SEIFA disadvantage decile (1 = most disadvantaged) | Integer | ABS 2021 |
| `seifa_group` | Grouped label: Most / Middle / Least disadvantaged | String | Derived |
| `seifa_score` | SEIFA Index of Relative Socioeconomic Disadvantage score | Integer | ABS 2021 |
| `squeeze_score` | Weighted score combining SEIFA, rent, and volume | Decimal | Derived |
| `squeezed_flag` | Squeezed if low SEIFA + high rent + high volume; else Not squeezed | String | Derived |
| `state` | State name (New South Wales) | String | Derived |
| `suburb` | Primary suburb name for postcode | String | NSW Fair Trading |
| `teacher_affordable` | Affordable / Unaffordable at $612/wk teacher salary | String | Derived |
| `total_lodgements` | Total rental bond lodgements | Integer | NSW Fair Trading |
| `total_refunds` | Total bond refunds processed | Integer | NSW Fair Trading |
| `weekly_bond_estimate` | Estimated weekly bond value (median rent × 4) | Decimal | Derived |

---

## Credits

### Data Sources

| Source | Description | URL |
|---|---|---|
| NSW Fair Trading | Rental bond lodgement and refund records, 2025 | data.nsw.gov.au — add exact dataset URL |
| Australian Bureau of Statistics (ABS) | SEIFA 2021 — Socio-Economic Indexes for Areas | abs.gov.au/statistics/people/people-and-communities/socio-economic-indexes-areas-seifa-australia/2021 |
| Mapbox / OpenStreetMap | Base map tiles used in the Tableau geography map | Via Tableau built-in map provider |
| NSW Health | Level 1 nurse weekly salary ($480/wk) — essential worker affordability curve | Add source URL |
| NSW Department of Education | Year 3 teacher weekly salary ($612/wk) — essential worker affordability curve | Add source URL |
| NSW Police Force | Police constable weekly salary ($770/wk) — essential worker affordability curve | Add source URL |

### Tools

| Tool | Purpose |
|---|---|
| Python 3 / pandas | Data cleaning, joining, and aggregation |
| Tableau Public | Dashboard creation and publication |
| GitHub | Version control and project repository |

### Group Members

| Name | Role |
|---|---|
| Aaron Reji | Data Architect |
| Vandana Rajagopal | Presentation and Documentation Contributor |
| Md Abtab Karim Kabbo | Dashboard Builder and Data Storyteller |
| Intesar Hassan Nahin | Orator and Presentation Contributor |
| Nishaanth Govindaraj | Insight Analyst |
| Harsh Javia | Project Manager and Data Narrative Contributor |
| Aditya Damodara | Dashboard Builder |

---
## Miro Board

Our project planning, EDA notes, and storyboard are documented on our group Miro board.

**Link:** [Miro Board](https://miro.com/app/board/uXjVGgF4rd8=/)
