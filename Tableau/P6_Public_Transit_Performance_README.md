# Public Transit Performance Analysis

## Project Overview

This project analyzes **New York City MTA bus ridership during 2025** using Tableau. The objective was to transform public transit ridership data into an interactive analytical dashboard that highlights route performance, demand patterns, peak periods, weekday versus weekend behavior, and stop-level ridership.

The project demonstrates an end-to-end analytics workflow, including:

- Public data acquisition
- Data aggregation and preparation
- Data quality validation
- Tableau data modeling
- Calculated fields
- Time-series analysis
- Route performance analysis
- Trip-type analysis
- Demand heatmaps
- Geographic visualization
- Interactive dashboard design
- Business-oriented insight generation

The project was intentionally designed to demonstrate **Tableau as the primary analytical and visualization tool** while maintaining a reproducible and defensible data-preparation process.

---

## Business Questions

The analysis was designed to answer the following questions:

1. How does bus ridership change throughout 2025?
2. Which routes have the highest ridership?
3. How does ridership differ between weekdays and weekends?
4. What hours experience the greatest demand?
5. How does demand vary by day of the week?
6. How does ridership differ across peak periods?
7. How does route demand differ by trip type?
8. Do different high-ridership routes have different demand patterns?
9. Which stops have the greatest observed ridership?
10. What operational patterns could inform transit scheduling and service planning?

---

# Data Sources

The project uses official datasets published through the **New York State Open Data platform**.

## 1. MTA Bus Stop Ridership Estimates — Beginning 2024

Dataset ID: `fvdm-uavx`

The original dataset contains bus stop ridership estimates with fields including:

- Month
- Weekday
- Day Type
- Hour
- Route ID
- Trip Type
- Stop ID
- Direction ID
- Stop Sequence
- Estimated Ridership

The analysis period was restricted to **January–December 2025**.

The original 2025 data contained approximately **36.95 million records**, which was too large to work with efficiently as an unaggregated Tableau dataset.

Instead of downloading and processing the full raw dataset locally, the data was aggregated through the Socrata API before being imported into Tableau.

---

## 2. MTA Bus Stop Performance Dataset

A separate stop-level aggregation was created from the same official ridership source.

The data was aggregated by:

- Month
- Route ID
- Trip Type
- Stop ID
- Direction ID

with estimated ridership summed within each grouping.

Final dataset:

**`MTA_Bus_Stop_Performance_2025.csv`**

Dataset characteristics:

- **306,812 records**
- **12 months**
- **383 routes**
- **15,948 unique stops**
- **2 directions**
- **5 trip types**

This dataset was used for stop-level analysis and the geographic ridership map.

---

## 3. MTA Bus Stops Lookup

Dataset ID: `2ucp-7wg5`

The official MTA Bus Stops dataset was used to provide contextual information for the stop-level analysis, including:

- Stop ID
- Stop name
- Route ID
- Route short name
- Route long name
- Direction
- Latitude
- Longitude
- Valid-from date
- Valid-to date
- In-effect status

A **December 31, 2025 snapshot** was extracted for use as the Tableau stop lookup.

Final lookup:

**`MTA_Stop_Lookup_2025.json`**

The snapshot contained:

- **22,924 records**
- **13,645 unique stops**
- **369 unique routes**
- 2 directions

---

# Data Preparation

## Main Transit Performance Dataset

The main dataset was aggregated using the following analytical grain:

> Month + Weekday + Day Type + Hour + Route ID + Trip Type

Estimated ridership was summed within each grouping.

The resulting dataset was:

**`P6_MTA_Transit_Performance_2025.csv`**

with:

- **581,460 rows**
- **7 columns**
- **12 months**
- **383 routes**
- **5 trip types**

Fields:

| Field | Description |
|---|---|
| Month | Month associated with the ridership observation |
| Weekday | Day of week |
| Day Type | Weekday or weekend classification |
| Hour | Hour of day |
| Route ID | MTA route identifier |
| Trip Type | Service type |
| Total Ridership | Aggregated estimated ridership |

---

# Data Validation

Several validation checks were performed before using the data in Tableau.

## Main Dataset

Validation results:

| Check | Result |
|---|---:|
| Records | 581,460 |
| Months | 12 |
| Routes | 383 |
| Trip types | 5 |
| Duplicate analytical keys | 0 |
| Negative ridership values | 0 |
| Missing ridership values | 234,957 |
| Missing ridership percentage | ~40.4% |

The missing ridership values were **not replaced with zero**.

This distinction is important because a missing value does not necessarily mean that ridership was zero. Replacing missing observations with zero could artificially reduce calculated totals and distort comparisons.

Trip-type missingness varied:

| Trip Type | Missing Ridership |
|---|---:|
| Local | ~45.1% |
| Express | ~2.4% |
| Limited | ~42.9% |
| Select | ~72.1% |
| Non-GTFS Shuttle | ~35.0% |

---

## Stop-Level Dataset

Validation results:

| Check | Result |
|---|---:|
| Records | 306,812 |
| Months | 12 |
| Routes | 383 |
| Unique stops | 15,948 |
| Directions | 2 |
| Duplicate analytical keys | 0 |
| Negative ridership values | 0 |
| Missing route IDs | 0 |
| Missing stop IDs | 0 |
| Missing months | 0 |
| Missing ridership | 1,949 |
| Missing ridership percentage | ~0.64% |
| Zero-ridership records | 17,255 |

Again, missing ridership values were preserved as missing rather than automatically converted to zero.

---

# Stop Data Reconciliation

A dedicated reconciliation step was performed between the **stop-performance data** and the **MTA stop lookup data** before the lookup information was used for geographic visualization and stop-level labeling.

The purpose of the reconciliation was to determine whether the stop identifiers in the performance dataset could be reliably matched to the stop-name and geographic information in the lookup.

The reconciliation used the combined key:

> **Route ID + Stop ID + Direction ID**

This was important because matching on **Stop ID alone** was not considered sufficient for the historical data.

The reconciliation results were:

| Reconciliation Check | Result |
|---|---:|
| Stop-performance records | 306,812 |
| Stop lookup records | 22,924 |
| Performance records with lookup match | 272,609 |
| Performance records without lookup match | 34,203 |
| Match coverage | **88.85%** |
| Unmatched performance records | **11.15%** |
| Unique performance combinations | 28,113 |
| Unique combinations with match | 22,759 |
| Unique combinations without match | 5,354 |

The reconciliation confirmed that **272,609 of the 306,812 stop-performance records could be matched to the December 31, 2025 stop lookup**, providing an **88.85% record-level match rate**.

The unmatched records were **retained** rather than removed.

This was important because the performance dataset covers the entire 2025 period, while the lookup represents only a **December 31, 2025 snapshot**. Historical stop configurations may therefore differ from the end-of-year lookup.

The reconciliation established the coverage and limitation of the lookup before it was used in Tableau.

---

# Stop Lookup Validation

The December 31, 2025 lookup itself was also validated.

Key checks included:

- Unique Route ID + Stop ID + Direction ID combinations
- Route and stop identifier completeness
- Direction values
- Latitude and longitude validity
- Missing route-name attributes

Results included:

- **22,924 lookup records**
- No duplicate Route ID + Stop ID + Direction ID keys
- Route ID, Stop ID, Stop Name, Direction ID, Direction, Latitude and Longitude were complete
- 6 missing Route Short Name values
- 6 missing Route Long Name values
- 770 missing Route Description values
- Coordinate conversion failures: **0**

This established that the lookup could safely provide stop names and geographic coordinates for matched records.

---

# Historical Stop Lookup Limitation

The stop lookup was intentionally treated as a **December 31, 2025 snapshot**, rather than attempting to reconstruct the exact stop configuration for every month of 2025.

Because the stop-performance dataset covers the entire year, some historical records do not match the end-of-year lookup.

The unmatched records were therefore **not discarded**.

The lookup was used primarily to provide:

- Stop names
- Coordinates
- Direction information
- Route context
- Mapping information

This means the map represents stop locations available in the December 31, 2025 lookup snapshot and should not be interpreted as a perfect historical reconstruction of every stop configuration throughout 2025.

---

# Tableau Data Preparation

Three separate Tableau data sources were used.

### 1. `P6_MTA_Transit_Performance_2025.csv`

Used for:

- Transit Overview
- Route Performance
- Demand Patterns
- Route analysis
- Time-based analysis

### 2. `MTA_Bus_Stop_Performance_2025.csv`

Used for:

- Stop-level analysis
- Geographic ridership analysis

### 3. `MTA_Stop_Lookup_2025.json`

Used as a secondary source for:

- Stop names
- Coordinates
- Directions
- Route context

The stop-performance dataset and stop lookup were combined in Tableau through **data blending** using:

- Direction ID
- Route ID
- Stop ID

Month was intentionally **not** used as a blend key because the lookup represents a December 31, 2025 snapshot rather than a month-by-month historical lookup.

The prior reconciliation established that this blend would have an **88.85% record-level match coverage** against the stop-performance dataset.

---

# Calculated Fields

## Peak Period

A calculated field was created to group hours into operational demand periods:

```text
IF [Hour] >= 6 AND [Hour] <= 9 THEN "Morning Peak"
ELSEIF [Hour] >= 10 AND [Hour] <= 15 THEN "Midday"
ELSEIF [Hour] >= 16 AND [Hour] <= 19 THEN "Evening Peak"
ELSE "Off-Peak"
END
```

Categories:

- Morning Peak
- Midday
- Evening Peak
- Off-Peak

## Weekday Type

The source contained lowercase values for day type, so the calculation normalized the values:

```text
IF LOWER([Day Type]) = "weekday" THEN "Weekday"
ELSEIF LOWER([Day Type]) = "weekend" THEN "Weekend"
END
```

## Peak Period Order

A numeric ordering field was also created:

```text
CASE [Peak Period]
    WHEN "Morning Peak" THEN 1
    WHEN "Midday" THEN 2
    WHEN "Evening Peak" THEN 3
    WHEN "Off-Peak" THEN 4
END
```

---

# Tableau Worksheets

The following worksheets were created.

## 1. Monthly Ridership Trend

Displays total estimated ridership across the 12 months of 2025.

**Chart type:** Line chart

**Title:**

> Monthly Transit Ridership — 2025

---

## 2. Ridership by Weekday Type

Compares total ridership between weekdays and weekends.

**Chart type:** Bar chart

---

## 3. Ridership by Peak Period

Compares total ridership across the four defined peak-period categories.

**Chart type:** Bar chart

---

## 4. Top Routes by Ridership

Identifies the ten routes with the highest displayed ridership.

**Chart type:** Horizontal bar chart

**Filter:** Top 10 routes by SUM(Total Ridership)

---

## 5. Ridership by Hour

Shows total estimated ridership across all 24 hours.

**Chart type:** Line chart

The Hour field was treated as a discrete dimension rather than an aggregated measure so that each hour could be displayed individually.

**Title:**

> Hourly Transit Ridership — 2025

---

## 6. Ridership by Day of Week

Compares ridership across the days of the week.

**Chart type:** Bar chart

---

## 7. Route Ridership by Trip Type

Shows the top routes while breaking ridership down by trip type.

**Chart type:** Bar chart

**Filters:**

- Route ID
- Trip Type

---

## 8. Ridership Heatmap — Day and Hour

Shows ridership intensity by day of week and hour.

**Chart type:** Heatmap

**Dimensions:**

- Weekday
- Hour

**Measure:**

- Total Ridership

---

## 9. Stop Ridership Map — 2025

Maps stop-level ridership using latitude and longitude from the stop lookup.

Map details include:

- Stop ID
- Stop name
- Route ID
- Direction
- Total ridership

Interactive filters allow route and trip-type exploration.

Only records with available lookup coordinates can be represented geographically.

---

## 10. Route Performance by Trip Type

Shows the top ten routes and their trip-type composition.

**Chart type:** Bar chart

**Title:**

> Top 10 Routes by Ridership & Trip Type — 2025

---

## 11. Weekday vs Weekend Hourly Demand

Compares hourly demand patterns between weekdays and weekends.

**Chart type:** Line chart

This visualization was particularly useful for comparing individual routes.

---

# Dashboards

## Dashboard 1 — Transit Overview

**Title:**

> Transit Overview

The dashboard combines:

- Monthly ridership trend
- Weekday versus weekend ridership
- Top route performance

The dashboard provides a high-level view of overall transit demand and the routes contributing the greatest ridership.

---

## Dashboard 2 — Transit Route Performance — 2025

This dashboard focuses on route-level performance.

It includes:

- Top 10 route performance by trip type
- Weekday versus weekend hourly demand

Interactive filters:

- Route ID
- Trip Type

This allows individual routes to be examined without leaving the dashboard.

---

## Dashboard 3 — Transit Demand Patterns — 2025

This dashboard combines:

- Day/hour ridership heatmap
- Weekday versus weekend hourly demand
- Peak-period analysis
- Stop ridership map

The dashboard is intended to explore **when and where transit demand occurs**.

---

# Key Findings

## 1. Weekday demand is substantially higher than weekend demand

The dashboard shows:

| Day Type | Ridership |
|---|---:|
| Weekday | **75,895,392** |
| Weekend | **33,369,473** |

Weekday ridership is more than twice weekend ridership.

This indicates that the system experiences substantially greater demand during the weekday period represented by the dataset.

---

## 2. Overall hourly demand peaks during the weekday morning

The highest displayed weekday hourly demand was:

> **41,597 at 9 AM**

The highest displayed weekend hourly demand was:

> **16,423 at 3 PM**

The weekday peak is approximately **2.5 times** the weekend peak.

This demonstrates both a substantial difference in demand volume and a difference in the timing of peak demand.

---

## 3. Demand varies considerably by route

The highest-ridership route in the final dashboard was:

> **S62 — 871,904**

The other top routes included:

| Rank | Route | Ridership |
|---:|---|---:|
| 1 | S62 | 871,904 |
| 2 | SIM1C | 869,365 |
| 3 | S40 | 776,049 |
| 4 | M35 | 774,984 |
| 5 | B65 | 750,822 |
| 6 | Q36 | 746,026 |
| 7 | S51 | 702,192 |
| 8 | B48 | 685,833 |
| 9 | S61 | 666,130 |
| 10 | Q48 | 659,664 |

The relatively close ridership totals among several of the highest-ranking routes demonstrate that high demand is distributed across multiple routes rather than concentrated entirely in one service.

---

## 4. Peak-period totals require careful interpretation

The dashboard displayed:

| Peak Period | Ridership |
|---|---:|
| Morning Peak | 21,360,035 |
| Midday | 24,917,383 |
| Evening Peak | 19,224,775 |
| Off-Peak | 43,762,672 |

The Off-Peak category has the largest aggregate total.

However, this should **not** be interpreted as meaning that individual off-peak hours are necessarily busier than peak hours.

The reason is that the categories cover different numbers of hours:

- Morning Peak: 6–9 AM
- Midday: 10 AM–3 PM
- Evening Peak: 4–7 PM
- Off-Peak: 12–5 AM and 8–11 PM

Therefore, aggregate totals across these categories are affected by the number of hours included in each category.

The hourly visualization provides a more appropriate basis for identifying specific demand peaks.

---

## 5. S62 has a different demand pattern from the overall system

For S62:

- **Weekday peak:** 43,586 at 11 AM
- **Weekend peak:** 14,328 at 5 PM

S62's weekday peak is approximately three times its weekend peak.

The route is a **Local** service.

This demonstrates that an individual high-ridership route does not necessarily follow the same hourly demand pattern as the overall transit system.

---

## 6. SIM1C demonstrates a substantially later demand peak

For SIM1C:

- **Weekday peak:** 42,818 at 10 PM
- **Weekend peak:** 16,119 at 8 PM

SIM1C is an **Express** service.

Its weekday peak occurring at 10 PM contrasts sharply with the overall weekday system peak at 9 AM and the S62 weekday peak at 11 AM.

This illustrates why route-level analysis is important when evaluating transit demand rather than relying exclusively on system-wide averages.

---

## 7. Q36 is primarily Local service

Q36 displayed:

| Trip Type | Ridership |
|---|---:|
| Local | 646,783 |
| Limited | 99,243 |
| **Total** | **746,026** |

Limited service accounts for approximately **13.3%** of Q36's displayed ridership.

The route is therefore predominantly represented by Local ridership in the dashboard, with a smaller but meaningful Limited component.

---

## 8. Demand differs between weekdays and weekends

The weekday/weekend analysis shows that demand is not simply a scaled version of the same daily pattern.

The overall peaks occur at different times:

- Weekday: **9 AM — 41,597**
- Weekend: **3 PM — 16,423**

Individual routes also show different peak timing, as demonstrated by S62 and SIM1C.

This suggests that demand analysis should consider both **time of day and route characteristics**.

---

# Business Implications

The findings provide several potential operational considerations.

### Service scheduling

The strong weekday demand profile suggests that service planning should account for substantially higher weekday demand than weekend demand.

### Time-of-day planning

The hourly analysis demonstrates that demand changes significantly throughout the day. Service frequency decisions should therefore consider specific hourly patterns rather than relying solely on daily totals.

### Route-specific planning

S62 and SIM1C demonstrate that high-ridership routes can have very different peak times.

This supports evaluating routes individually when considering:

- Frequency
- Scheduling
- Capacity
- Resource allocation

### Weekend service planning

Weekend demand is substantially lower overall, but it also peaks at a different time than weekday demand.

This suggests that weekend scheduling should not simply mirror weekday schedules.

### Stop-level analysis

The stop map provides a geographic perspective that can help identify areas with higher observed stop-level ridership and support further investigation of service demand by location.

---

# Important Limitations

## Estimated ridership

The analysis uses **estimated ridership**, not direct passenger counts.

Therefore, the results should be interpreted as analysis of the published MTA ridership estimates.

## Missing ridership

The main dataset contains **234,957 missing ridership observations**, representing approximately 40.4% of records.

These values were intentionally preserved as missing rather than converted to zero.

Consequently, analyses based on ridership totals should be understood within the limitations of the available observations.

## Stop lookup snapshot

The stop lookup represents a **December 31, 2025 snapshot**.

It does not reconstruct the exact stop configuration for every month of 2025.

The stop-performance dataset had an **88.85% record-level match** to this lookup using Route ID + Stop ID + Direction ID.

The unmatched 11.15% of performance records were retained in the underlying data rather than discarded.

The map therefore should not be interpreted as a complete historical map of every stop configuration throughout 2025.

## Peak-period aggregation

The four peak-period categories contain different numbers of hours.

Therefore, comparing their aggregate totals directly can be misleading.

Hourly analysis provides a better measure for identifying specific demand peaks.

## Route-level comparisons

Different routes can have different service structures and trip types.

Ridership totals therefore provide a measure of observed demand but should not automatically be interpreted as measures of service efficiency or route productivity.

---

# Skills Demonstrated

## Tableau

- Data connection
- Data preparation
- Dimensions and measures
- Calculated fields
- Filters
- Aggregations
- Sorting
- Top-N filtering
- Time-series visualization
- Heatmaps
- Geographic mapping
- Data blending
- Interactive dashboards
- Dashboard filtering
- Tooltips
- KPI-style summary views

## Data Analysis

- Data validation
- Data reconciliation
- Missing-value analysis
- Aggregation
- Trend analysis
- Time-series analysis
- Route comparison
- Day-of-week analysis
- Peak-period analysis
- Trip-type analysis
- Geographic analysis
- Comparative analysis

## Data Quality

- Duplicate-key validation
- Null-value assessment
- Negative-value checks
- Record-count validation
- Lookup reconciliation
- Lookup matching
- Match-rate calculation
- Coverage analysis
- Historical snapshot limitations

## Analytical Communication

- Business-question framing
- Dashboard design
- KPI presentation
- Insight development
- Operational interpretation
- Documentation of assumptions and limitations

---

# Project Outcome

This project transformed large public transit ridership data into an interactive Tableau analysis covering **system trends, route performance, demand timing, trip types, weekday/weekend differences, and stop-level geography**.

The workflow included not only visualization and analysis, but also **data validation and reconciliation** to establish whether the stop-performance records could be reliably associated with stop names and geographic attributes.

The stop reconciliation achieved an **88.85% record-level match** using Route ID + Stop ID + Direction ID. The remaining records were retained because the lookup represented a December 31, 2025 snapshot rather than a complete historical stop reference.

The analysis demonstrates that transit demand varies substantially by:

- Route
- Hour
- Day type
- Day of week
- Trip type
- Location

The project also demonstrates the importance of documenting data-quality limitations. Missing ridership values and the historical stop-lookup limitation were explicitly preserved and documented rather than hidden or converted into unsupported assumptions.

The final Tableau dashboards provide an interactive analytical interface for exploring these patterns and demonstrate practical skills in **Tableau visualization, data preparation, reconciliation, validation, dashboard development, and business-oriented data analysis**.
