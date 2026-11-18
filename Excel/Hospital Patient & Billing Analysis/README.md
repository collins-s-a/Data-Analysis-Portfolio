# Hospital Patient & Billing Analysis

An Excel-based data analytics project exploring hospital patient
demographics, departmental activity, treatment costs, insurance-provider
activity, and payment status through data profiling, PivotTables, an
interactive dashboard, and business recommendations.

> **Project type:** Portfolio / learning project\
> **Tool:** Microsoft Excel 2019\
> **Dataset:** 500 synthetic hospital visit records\
> **Period covered:** 2024--2025\
> **Currency:** Canadian dollars (CAD)

------------------------------------------------------------------------

## Table of Contents

-   [Project Overview](#project-overview)
-   [Business Objectives](#business-objectives)
-   [Dataset Overview](#dataset-overview)
-   [Tools and Excel Techniques](#tools-and-excel-techniques)
-   [Analysis Workflow](#analysis-workflow)
-   [Key Findings](#key-findings)
-   [Business Insights and
    Recommendations](#business-insights-and-recommendations)
-   [Dashboard Preview](#dashboard-preview)
-   [Data Limitations](#data-limitations)
-   [Repository Contents](#repository-contents)
-   [How to Explore the Project](#how-to-explore-the-project)
-   [Disclaimer](#disclaimer)

------------------------------------------------------------------------

## Project Overview

The goal of this project was to analyse hospital patient and billing
data and turn the results into practical insights that could support
operational and financial decision-making.

Using Excel 2019, I profiled the dataset, analysed patient visit
patterns, compared treatment costs across departments, examined
insurance-provider activity and reviewed payment statuses. I then
developed an interactive dashboard using PivotTables, charts, KPI cards
and slicers.

The project also focused on identifying data gaps. For example, the
dataset recorded whether a visit was paid, partially paid or pending,
but did not include the actual amount paid. This meant that the exact
outstanding balance could not be calculated. Identifying this limitation
helped shape the project's recommendations for improving data
collection.

## Business Objectives

The analysis was designed to answer questions such as:

1.  What does the patient population look like in terms of age and
    gender?
2.  Which departments record the highest and lowest visit volumes?
3.  Which departments have the highest total and average treatment
    costs?
4.  How are treatment costs distributed across payment statuses?
5.  Which insurance provider is associated with the highest visit volume
    and treatment costs?
6.  What operational or financial questions require additional data to
    answer reliably?

## Dataset Overview

The main dataset contains **500 records and 11 columns**. Each row
represents a **patient visit**.

  Column                 Description
  ---------------------- -----------------------------------------------
  `Patient_ID`           Patient identifier
  `Patient_Name`         Patient name field in the synthetic dataset
  `Age`                  Patient age
  `Gender`               Recorded gender category
  `Department`           Hospital department associated with the visit
  `Diagnosis`            Recorded diagnosis
  `Admission_Date`       Visit admission date
  `Discharge_Date`       Visit discharge date
  `Insurance_Provider`   Recorded insurance provider
  `Treatment_Cost`       Recorded treatment cost in CAD
  `Payment_Status`       Paid, Partially Paid or Pending

The workbook also includes an insurance lookup sheet and a data
dictionary.

**Important:** The dataset is synthetic and intended for learning and
portfolio demonstration. It does not represent actual patients or a real
hospital.

## Tools and Excel Techniques

-   **Microsoft Excel 2019**
-   PivotTables and PivotCharts
-   Slicers and Report Connections
-   KPI cards linked to PivotTable results using `GETPIVOTDATA`
-   `COUNTIF`, `COUNTIFS`, `COUNTA`, `AVERAGE`, `SUMIF` and other
    standard Excel functions
-   Data profiling and validation checks
-   Dashboard layout and data visualization

The workbook was designed to remain compatible with Excel 2019; it does
not rely on newer dynamic-array functions such as `UNIQUE` or `SORT`.

## Analysis Workflow

### 1. Data Acquisition and Initial Profiling

-   Reviewed the workbook structure, fields and record grain.
-   Checked record counts, missing values, duplicates, data types, date
    validity, cost values and category consistency.
-   Considered the distinction between a duplicate record and a
    legitimate repeat visit.

The initial review reported 500 records and no identified missing
values, duplicate full records or obvious invalid date and
treatment-cost values.

### 2. Patient Population Analysis

-   Calculated total visits and average patient age.
-   Examined age groups and gender distribution.
-   Compared visit counts across hospital departments.
-   Explored department activity within age groups.

### 3. Financial and Billing Analysis

-   Calculated total, average, highest and lowest treatment costs.
-   Compared total and average treatment costs by department.
-   Examined visit counts and associated treatment costs by insurance
    provider.
-   Analysed payment status by visit count, treatment cost and share of
    total treatment costs.

### 4. Interactive Dashboard Development

Built an Excel dashboard containing:

**KPI cards** - Total Visits - Unique Patients - Average Patient Age -
Total Treatment Costs - Pending Treatment Costs - Average Treatment Cost

**Charts** - Patient Visits by Department - Patient Visits by Gender -
Patient Visits by Age Group - Treatment Costs by Department - Treatment
Costs by Payment Status

**Interactive slicers** - Department - Gender - Insurance Provider -
Payment Status

The slicers are connected to the relevant PivotTables so users can
explore the dashboard by category.

## Key Findings

  -----------------------------------------------------------------------
  Area                                Finding
  ----------------------------------- -----------------------------------
  Total visits                        500

  Average patient age                 46.26 years

  Largest age group                   11--20 years

  Visits from patients aged 60+       35%

  Gender distribution                 Female: 50.40%; Male: 46.80%;
                                      Non-binary: 2.80%

  Busiest department                  Pediatrics --- 16.20% of visits

  Least busy department               Cardiology

  Total treatment costs               \$1,408,788.27

  Average treatment cost              \$2,817.58

  Highest treatment cost for one      \$9,119.54
  visit                               

  Lowest treatment cost for one visit \$473.71

  Highest total and average           Oncology
  departmental treatment costs        

  Lowest departmental total treatment General Medicine
  costs                               

  Highest visit count and associated  BlueCross
  treatment costs by insurer          
  -----------------------------------------------------------------------

### Payment Status Breakdown

  -------------------------------------------------------------------------
  Payment status             Visits      Treatment costs     Share of total
                                                            treatment costs
  -------------- ------------------ -------------------- ------------------
  Paid                          281         \$797,918.47             56.64%

  Partially Paid                 66         \$179,385.42             12.73%

  Pending                       153         \$431,484.38             30.63%

  **Total**                 **500**   **\$1,408,788.27**        **100.00%**
  -------------------------------------------------------------------------

Treatment costs associated with partially paid and pending visits total
**\$610,869.80 (43.36%)**.

This is the value of treatment costs linked to those payment statuses.
It is **not** a confirmed outstanding receivable balance because the
dataset does not record the amount actually paid against each visit.

## Business Insights and Recommendations

### 1. Older adults represent a substantial share of visit activity

Patients aged 60 and above account for 35% of visits.

**Recommendation:** Review diagnoses and treatment patterns for this age
group, assess staff training and accessibility needs, and investigate
whether current services and resources are appropriate. Additional
visit-history data would be needed to assess repeat visits and
readmissions.

### 2. Oncology is a major contributor to recorded treatment costs

Oncology has the highest total and average treatment costs.

**Recommendation:** Investigate visit volumes, treatment complexity,
staffing, medication, equipment and resource requirements before making
operational decisions. Total and average costs together help distinguish
overall cost impact from typical cost per visit.

This finding concerns treatment costs; it does not establish
departmental profitability or net financial contribution.

### 3. Partially paid and pending visits warrant billing follow-up

A combined \$610,869.80 in treatment costs is associated with partially
paid and pending visits.

**Recommendation:** Review insurance eligibility and coverage, including
limits, exclusions, deductibles and co-payments. Strengthen claim
follow-up and consider appropriate payment arrangements for uninsured or
underinsured patients. Add actual payment and balance fields to
calculate receivables accurately.

### 4. BlueCross activity merits further evaluation

BlueCross has the highest visit count and highest associated treatment
costs.

**Recommendation:** Evaluate coverage and affordability across diagnoses
and treatment types. Review claim approval and rejection rates,
reimbursement timelines and patient out-of-pocket costs. This could help
identify opportunities to improve coverage or strengthen the insurance
partnership.

### 5. Improve the completeness and quality of collected data

Several potentially valuable analyses were limited by missing fields.

**Recommendation:** Expand data collection to include:

-   Actual amount paid and outstanding balance.
-   Insurance coverage details, deductibles, co-payments and exclusions.
-   Claim submission, approval, rejection reasons and reimbursement
    dates.
-   A consistent patient identifier and visit history.
-   More detailed diagnosis and treatment information.
-   Staffing, equipment and resource-utilization information.
-   Relevant clinical outcomes and discharge/follow-up information.

**How to measure success:** Track data completeness and accuracy, then
assess whether the hospital can reliably calculate previously
unavailable metrics such as exact outstanding receivables, claim
rejection rates and repeat-visit rates. The value of better data is
realized when validated insights support better operational and
financial decisions.

## Dashboard Preview

Add screenshots of the **actual Excel dashboard** to the `dashboard/`
folder, then embed them here. These images allow visitors to preview the
work without downloading the workbook.

```{=html}
<!-- Replace the paths below if your screenshot filenames differ. -->
```
![Hospital dashboard overview](Dashboard Screenshots\DSS1.png)
![Hospital dashboard overview](Dashboard Screenshots\DSS2.png)
![Hospital dashboard overview](Dashboard Screenshots\DSS3.png)

*Optional: include a second screenshot showing the dashboard after
applying one or more slicer filters.*

![Hospital dashboard filtered view](dashboard/dashboard_filtered.png)

## Data Limitations

The results should be interpreted with the following limitations in
mind:

-   The dataset is synthetic and contains 500 visit records; findings
    should not be generalized to a real hospital.
-   The dataset does not include `Amount_Paid` or `Outstanding_Balance`,
    so exact outstanding receivables cannot be calculated.
-   Insurance coverage details, claim outcomes and reimbursement
    timelines are unavailable.
-   Patient visit history is insufficient to establish repeat visits or
    readmission patterns.
-   The available diagnosis and treatment detail may not fully explain
    departmental cost differences.
-   Staffing, capacity, equipment utilization and clinical outcomes are
    not included.
-   Treatment cost is not the same as revenue, reimbursement, profit or
    financial contribution.
-   Demographic percentages describe visit records, not necessarily
    unique individuals if repeat visits occur.

## Repository Contents

The repository is intended to contain the following files:

``` text
hospital-patient-billing-analysis/
│
├── README.md
├── data/
│   ├── hospital_patient_billing_raw.csv
│   └── data_dictionary.md
├── excel/
│   └── Hospital_Patient_Billing_Analysis.xlsx
├── dashboard/
│   ├── dashboard_overview.png
│   └── dashboard_filtered.png
├── report/
│   └── Hospital_Patient_and_Billing_Analysis_Report.pdf
└── insights/
    └── key_business_insights.md
```

If you use different filenames or choose not to include an optional
file, update this section to match the repository exactly.

## How to Explore the Project

1.  Open the dashboard screenshots in the `dashboard/` folder for a
    quick overview.
2.  Read the PDF report in `report/` for the full analysis,
    interpretation and recommendations.
3.  Download the Excel workbook in `excel/` to inspect the raw data,
    PivotTables, formulas, slicers and interactive dashboard.
4.  Review the data dictionary for field definitions.

## Disclaimer

This is a synthetic-data portfolio project created for educational and
demonstration purposes. It is not medical advice, a clinical assessment,
a financial audit or an evaluation of an actual healthcare organization.
