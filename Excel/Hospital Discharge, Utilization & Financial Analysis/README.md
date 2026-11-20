# P1 — Hospital Discharge & Financial Analysis

## Project Overview

This project analyzes a 20,000-record sample of the **New York State SPARCS 2024 Hospital Inpatient Discharges (De-Identified)** dataset using Microsoft Excel.

The goal was to demonstrate an end-to-end analytics workflow: obtaining a public healthcare dataset, extracting a manageable sample, profiling and validating the data, performing exploratory and business analysis, building PivotTables and calculations, and presenting findings in an interactive Excel dashboard.

> **Important:** The dataset contains hospital discharge records, not necessarily unique patients. Reported Total Charges and Total Costs are hospital-level financial measures and should not be interpreted as revenue collected, patient payments, or profit.

---

## Business Objective

The analysis was designed to answer questions such as:

- What types of admissions drive inpatient discharge volume?
- How does patient age relate to length of stay and financial measures?
- How does severity of illness relate to utilization and financial measures?
- How do medical and surgical cases differ?
- Which facilities have the highest discharge volume?
- How do payer categories differ in volume and average financial measures?
- What data-quality issues and outliers should be considered before drawing conclusions?

---

## Data Source

**Dataset:** NYS SPARCS 2024 Hospital Inpatient Discharges (De-Identified)

**Publisher:** New York State Department of Health

**Official dataset page:** https://health.data.ny.gov/Health/Hospital-Inpatient-Discharges-SPARCS-De-Identified/sf4k-39ay

The source dataset is substantially larger than a practical Excel working file. Rather than attempting to load the entire file into the workbook, a random 20,000-record sample was extracted using Excel Power Query.

---

## Project Workflow

### 1. Obtain the source data

The official SPARCS 2024 inpatient discharge dataset was selected because it contains patient demographic, admission, diagnosis, procedure, severity, payer, utilization, and financial fields that support a broad healthcare analytics case study.

### 2. Import the source through Power Query

The source CSV was connected through:

**Excel → Data → Get Data → From File → From Text/CSV → Transform Data**

This allowed the large source file to be handled through Power Query instead of manually opening the entire dataset.

### 3. Create a 20,000-record sample

A random sampling workflow was used in Power Query:

1. Add a Custom Column named `Random_Sample`.
2. Use `Number.Random()` to generate a random value for each record.
3. Sort `Random_Sample` ascending.
4. Keep the top 20,000 rows.
5. Remove the helper column.
6. Rename the query to `SPARCS_2024_Sample`.
7. Load the result to an Excel table on the `Raw_Data` worksheet.

**Reproducibility note:** `Number.Random()` can generate a different sample when the query is refreshed. The final analytical workbook therefore represents the sampled 20,000 records used for this project; refreshing the query may produce a different sample.

---

## Data Structure

The analytical sample contains **20,000 discharge records and 33 fields**.

Key field groups include:

- **Facility:** Health Service Area, Hospital County, Operating Certificate Number, Permanent Facility Id, Facility Name
- **Demographics:** Age Group, Zip Code, Gender, Race, Ethnicity
- **Utilization:** Length of Stay, Type of Admission, Patient Disposition, Emergency Department Indicator
- **Diagnosis/Procedure:** CCSR Diagnosis Code/Description, CCSR Procedure Code/Description
- **Clinical severity:** APR DRG, APR MDC, APR Severity of Illness, APR Risk of Mortality, Medical/Surgical classification
- **Payment:** Payment Typology 1, 2, and 3
- **Financial:** Total Charges, Total Costs
- **Other:** Discharge Year, Birth Weight

---

## Data Validation & Quality Checks

Before analysis, the sample was profiled for completeness, data types, categorical values, and potential outliers.

### Missing values

Notable missingness included:

| Field | Missing | Missing % |
|---|---:|---:|
| Zip Code | 1,031 | 5.16% |
| CCSR Procedure Code | 5,648 | 28.24% |
| CCSR Procedure Description | 5,648 | 28.24% |
| Payment Typology 2 | 11,575 | 57.88% |
| Payment Typology 3 | 17,913 | 89.57% |
| Birth Weight | 17,162 | 85.81% |

Birth Weight missingness was expected because the field is primarily applicable to neonate records and was not imputed.

Payment Typology 2 and 3 were retained as optional secondary payer fields; blanks were not treated as data errors.

### Facility completeness

A helper classification was used to check the four facility-identification fields:

- All four missing: **53 records (0.27%)**
- Partially missing: **0**
- Complete: **19,947 (99.74%)**

This indicates that the 53 records with facility information missing were consistently missing across all four facility-related fields rather than partially populated.

### Data type validation

Numeric validation was performed for key analytical fields including:

- Length of Stay
- Discharge Year
- APR Severity of Illness Code
- Birth Weight
- Total Charges
- Total Costs

The official data dictionary was used when determining source data types. For analytical purposes, Length of Stay was treated as numeric, with the source's 120+ aggregation rule documented.

### Categorical validation

Major categorical fields were checked for unexpected values, blanks, and frequency distributions, including:

- Gender
- Race
- Ethnicity
- Age Group
- Type of Admission
- Patient Disposition
- APR Severity of Illness
- APR Risk of Mortality
- Medical/Surgical classification
- Payment Typology 1
- Emergency Department Indicator

### Outlier assessment

IQR-based checks were performed on financial measures.

**Total Charges**
- Q1: $22,158.68
- Q3: $89,402.33
- Upper IQR threshold: $190,268.81
- Records above threshold: 1,811 (9.06%)

**Total Costs**
- Q1: $6,928.51
- Q3: $25,988.60
- Upper IQR threshold: $54,578.74
- Records above threshold: 1,853 (9.27%)

These records were **not automatically removed**. High values can represent legitimate high-acuity or resource-intensive cases.

---

## Analytical Approach

The analysis used Excel PivotTables, aggregation, filtering, calculated comparisons, and dashboard visualizations.

The main analytical dimensions were:

1. Admission type
2. Age group
3. Severity of illness
4. Medical vs. surgical classification
5. Facility
6. Payment typology
7. Emergency department indicator
8. Risk of mortality
9. Length of stay
10. Total charges and total costs

The dashboard was built with interactive slicers for:

- **Age Group**
- **Type of Admission**
- **Severity of Illness**

The Severity of Illness slicer excludes **Undetermined**, matching the dashboard chart because only seven records were classified as Undetermined.

---

# Dashboard Findings

The final dashboard contains four KPI cards, five analytical charts, three slicers, a Key Insights panel, and source/methodology notes.

### KPI summary

| KPI | Result |
|---|---:|
| Total Discharges | 20,000 |
| Average Length of Stay | 5.62 days |
| Average Total Charges | $88,216 |
| Average Total Costs | $25,017 |

### 1. Emergency admissions dominate discharge volume

Emergency admissions represented **63.1%** of sampled discharges.

This indicates that acute-care demand is a major driver of inpatient volume in the sample.

**Business implication:** Capacity planning and coordination between emergency and inpatient services are important operational considerations.

### 2. Ages 50–69 have the highest average charges

The 50–69 age group had:

- Average LOS: **6.40 days**
- Average charges: **$107,274.52**
- Average costs: **$31,144.65**

The 70+ group was second in both LOS and average financial measures.

**Business implication:** Older patient groups may require greater attention to discharge planning, care coordination, and resource utilization.

### 3. Extreme severity has dramatically higher resource requirements

| Severity | Avg. LOS | Avg. Charges | Avg. Costs |
|---|---:|---:|---:|
| Minor | 2.97 | $42,605 | $12,293 |
| Moderate | 4.81 | $65,162 | $18,993 |
| Major | 7.14 | $114,132 | $32,260 |
| Extreme | 16.72 | $329,864 | $90,108 |

The relationship is strong and consistent: increasing severity is associated with longer stays and higher financial measures.

**Business implication:** Severity can be used as a practical planning dimension for staffing, bed capacity, discharge planning, and resource allocation.

### 4. Surgical cases have substantially higher financial measures

Medical cases represented **77.84%** of the sample, while surgical cases represented **22.13%**.

| Classification | Avg. LOS | Avg. Charges | Avg. Costs |
|---|---:|---:|---:|
| Medical | 5.55 | $70,128 | $20,092 |
| Surgical | 5.88 | $151,942 | $42,364 |

Surgical cases had more than twice the average charges and costs of medical cases.

**Business implication:** Surgical services are a high-priority area for utilization, procedure-mix, and resource-efficiency analysis.

### 5. The top 10 facilities account for 20.2% of sampled discharges

The top 10 facilities by discharge volume accounted for **4,029 of 20,000 records**, or **20.15%**.

The highest-volume facility in the sample was Mount Sinai Hospital with **586 discharges**.

However, high volume did not necessarily mean the highest average financial measures. For example, NYP Columbia had the highest average charges among the top 10 facilities at approximately **$222,298**.

**Business implication:** Facility benchmarking should consider multiple measures rather than volume alone.

---

# Additional Findings Not Displayed on the Dashboard

These analyses were completed in the supporting PivotTables/analysis work but were not selected for the final dashboard.

## Risk of Mortality

| Risk of Mortality | Avg. LOS | Avg. Charges | Avg. Costs |
|---|---:|---:|---:|
| Minor | 3.98 | $53,140 | $15,524 |
| Moderate | 5.39 | $83,351 | $23,919 |
| Major | 7.92 | $133,891 | $36,814 |
| Extreme | 14.36 | $283,714 | $78,825 |

The same broad relationship seen with severity of illness appears for risk of mortality: higher-risk categories are associated with longer stays and higher financial measures.

## Emergency Department Indicator

- ED indicator = Yes: **59.23%**
- ED indicator = No: **40.77%**
- ED = Yes average LOS: **5.87 days**
- ED = No average LOS: **5.26 days**
- ED = Yes average charges: **$87,472**
- ED = No average charges: **$89,297**

An important distinction is that **Emergency Type of Admission (63.11%)** and **Emergency Department Indicator = Yes (59.23%)** are different fields and should not be treated as the same measure.

## Primary Payment Typology

| Primary Payment Type | Discharges | Share |
|---|---:|---:|
| Medicare | 7,499 | 37.50% |
| Medicaid | 6,276 | 31.38% |
| Private Health Insurance | 3,226 | 16.13% |
| Blue Cross/Blue Shield | 1,936 | 9.68% |

Medicare and Medicaid together represented **68.88%** of sampled discharges.

Average charges also varied across payer categories. Medicare averaged approximately **$104,907**, Medicaid **$78,998**, and Private Health Insurance **$86,190**.

**Interpretation:** Payer mix should be considered alongside patient mix, severity, and utilization when evaluating financial performance.

## Admission Type and Financial Measures

| Admission Type | Avg. Charges | Avg. Costs |
|---|---:|---:|
| Trauma | $141,226 | $40,178 |
| Elective | $96,361 | $29,045 |
| Urgent | $96,095 | $28,013 |
| Emergency | $87,789 | $24,978 |
| Newborn | $74,318 | $18,158 |
| Not Available | $66,767 | $17,827 |

Trauma had the highest average charges, but only **84 records (0.42%)** were classified as trauma. This small base means the result should be interpreted cautiously.

## Facility Benchmarking

Among the top 10 facilities by volume:

- NYP Columbia had the highest average charges at **$222,298**
- NYU Langone Hospitals averaged **$196,544**
- Montefiore Henry & Lucy Moses averaged **$205,813**
- Strong Memorial had much lower average charges at **$65,087**

The supporting analysis also calculated charge-to-cost ratios as a descriptive comparison. These ratios are **not profit margins** because Total Charges are not equivalent to collected revenue.

---

# Business Recommendations

### 1. Prioritize high-severity cases in resource planning

High-severity cases have substantially longer stays and higher financial measures. Severity-based forecasting can support staffing, bed capacity, care coordination, and discharge planning.

### 2. Examine surgical resource utilization

Surgical cases show substantially higher average charges and costs. A deeper analysis of procedure mix, operating-room utilization, LOS, and post-operative care could identify operational improvement opportunities.

### 3. Strengthen discharge planning for higher-utilization groups

Older and high-severity patients show higher average LOS and financial measures. Early discharge planning and coordinated post-discharge services could help manage capacity and reduce avoidable extended stays.

### 4. Improve emergency-to-inpatient capacity planning

Emergency admissions represent 63.1% of sampled discharges. Hospitals can use admission patterns and severity information to anticipate inpatient demand and manage bed capacity.

### 5. Benchmark facilities using multiple KPIs

Facility comparisons should combine discharge volume, LOS, severity, charges, costs, and case mix. Volume alone can create misleading comparisons.

### 6. Investigate high-value outliers rather than deleting them

High-charge and high-cost cases should be investigated by severity, diagnosis, procedure, facility, and admission type before being considered errors.

### 7. Incorporate payer mix into financial analysis

Medicare and Medicaid represent a large share of primary payment types. Payer mix should be evaluated alongside case mix and utilization rather than in isolation.

---

# Limitations

- The analysis uses a **20,000-record sample**, not the entire SPARCS 2024 dataset.
- The sample was generated using a random Power Query method and can change if the query is refreshed.
- Records represent **discharges**, not necessarily unique patients.
- Total Charges and Total Costs are hospital-level measures and are not equivalent to revenue, reimbursement, patient payments, or profit.
- Length of Stay includes a source-level aggregation rule where stays of 120 days or more are represented as **120+**.
- Several fields have meaningful missingness, including procedure, secondary/tertiary payer, and birth-weight fields.
- Outlier values were retained because high values may reflect legitimate high-acuity cases.
- Findings should be interpreted as patterns observed in the sample rather than definitive conclusions about all New York hospitals.

---

## Tools Used

- Microsoft Excel
- Excel Power Query
- Excel PivotTables
- Excel formulas
- Excel charts
- Excel slicers

---

## Portfolio Value

This project demonstrates an end-to-end analytics workflow:

**Data acquisition → Power Query sampling → Data profiling → Data quality validation → Exploratory analysis → PivotTables → Business analysis → Interactive dashboard → Recommendations**

It demonstrates practical skills in data preparation, validation, quantitative analysis, visualization, dashboard development, and communicating analytical findings to business stakeholders.

---


## Source

New York State Department of Health — SPARCS 2024 Hospital Inpatient Discharges (De-Identified)

https://health.data.ny.gov/Health/Hospital-Inpatient-Discharges-SPARCS-De-Identified/sf4k-39ay
