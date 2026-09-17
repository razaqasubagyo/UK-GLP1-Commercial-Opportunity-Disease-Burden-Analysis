# Data Sources

This project uses publicly available NHS datasets to analyse GLP-1 prescribing, registered population and diabetes burden across England.

Raw source files are not included in this repository due to file size and because the datasets are publicly available from the original providers.

---

## 1. English Prescribing Dataset

Source:

https://www.nhsbsa.nhs.uk/prescription-data/prescribing-data/english-prescribing-data-epd

Provider:

NHS Business Services Authority

Analysis period:

January 2025 to June 2026

Purpose:

Used as the primary prescribing fact dataset for analysing:

- GLP-1 prescription items
- prescribing trends
- molecule-level market share
- year-on-year growth
- Net Ingredient Cost
- Actual Cost
- practice-level geographic activity

Relevant fields include:

- YearMonth
- PracticeCode
- PracticeName
- ICBCode
- ICBName
- ChemicalSubstanceCode
- Molecule
- PresentationCode
- PresentationName
- Items
- Quantity
- NIC
- ActualCost
- SNOMEDCode

Monthly files were harmonised because the NHS prescribing schema changed during the analysis period.

January and February 2025 used an earlier field structure, while later monthly files used the revised schema.

The files were standardised and appended into a single Power BI fact table:

`Fact_Prescribing`

---

## 2. Patients Registered at a GP Practice

Source:

https://digital.nhs.uk/data-and-information/publications/statistical/patients-registered-at-a-gp-practice/june-2026

Provider:

NHS England / NHS Digital

Snapshot used:

June 2026

Purpose:

Used to create a population denominator for geographic prescribing analysis.

Relevant fields include:

- PracticeCode
- RegisteredPopulation

The source data was aggregated to GP practice level before being connected to the analytical model.

Final Power BI table:

`Practice_Population_202606`

This dataset supports the calculation of:

- Core Diabetes Items per 1,000 registered population
- Population Market Development Index (MDI)

---

## 3. Quality and Outcomes Framework Diabetes Data

Source:

https://digital.nhs.uk/data-and-information/publications/statistical/quality-and-outcomes-framework-achievement-prevalence-and-exceptions-data/2025-26

Provider:

NHS England / NHS Digital

Reporting period:

2025–26

Table used:

Diabetes / DM

Purpose:

Used to incorporate recorded diabetes burden into the geographic analysis.

Relevant fields include:

- PracticeCode
- PracticeName
- DiabetesRegister
- DiabetesPrevalence

Final Power BI table:

`QOF_Diabetes_202526`

This dataset supports the calculation of:

- Disease-Adjusted Utilisation Rate (DAUR)
- Disease-Adjusted Development Index (DADI)
- Denominator sensitivity analysis

---

## Market Definition

The prescribing extract focuses on the following GLP-1 receptor agonist molecules:

- Semaglutide
- Dulaglutide
- Liraglutide
- Exenatide
- Lixisenatide

Chemical-level filtering alone was not sufficient because some molecules are used across both diabetes and weight-management pathways.

A presentation-level product classification was therefore created.

Products were classified as:

- Type 2 Diabetes
- Weight Management
- Requires Review

Only presentations classified as Type 2 Diabetes were included in the core diabetes market analysis.

Examples of diabetes-focused products include:

- Ozempic
- Rybelsus
- Trulicity
- Victoza
- Byetta
- Bydureon
- Lyxumia

Examples of weight-management products excluded from the core diabetes analysis include:

- Wegovy
- Saxenda

---

## Data Preparation

Data transformation was completed using Power Query.

Key steps included:

- filtering prescribing data to selected GLP-1 molecules
- harmonising old and new prescribing schemas
- standardising column names
- converting monthly period fields into valid dates
- validating numeric data types
- correcting decimal and locale interpretation
- appending monthly prescribing files
- constructing presentation-level product classification
- creating product, date and practice dimensions
- aggregating registered population to practice level
- integrating QOF diabetes data using PracticeCode
- removing duplicate dimension keys
- handling missing denominator records
- excluding invalid denominator records only from the relevant normalised geographic measures

---

## Analytical Tables

The final Power BI model contains:

- `Fact_Prescribing`
- `Dim_Date`
- `Dim_Product`
- `Dim_Practice`
- `Practice_Population_202606`
- `QOF_Diabetes_202526`

The main relationship keys are:

- `YearMonth`
- `PresentationCode`
- `PracticeCode`

---

## Important Data Interpretation Notes

### Prescription Items Are Not Unique Patients

The prescribing dataset records prescription items rather than individual patients.

One patient may receive multiple items during the analysis period.

Therefore prescribing volume should not be interpreted as patient count.

### NIC Is Not Manufacturer Revenue

Net Ingredient Cost is used as an indicator of prescribing-associated financial scale.

It should not be interpreted as pharmaceutical manufacturer revenue, net sales or profit.

### Diabetes Register Is Not GLP-1 Eligibility

The QOF diabetes register represents recorded diabetes burden.

It does not identify the population clinically eligible for GLP-1 treatment.

### Disease-Adjusted Utilisation Is a Proxy

DAUR and DADI are comparative analytical measures.

They should not be interpreted as treatment penetration.

### Temporal Alignment Is Imperfect

The datasets represent different reporting structures:

- prescribing data: monthly
- QOF diabetes data: annual
- registered population: June 2026 snapshot

These datasets therefore do not represent perfectly contemporaneous measurements.

---

## Raw Data Availability

Raw data files are not stored in this repository.

Reasons include:

- large file sizes
- monthly prescribing datasets contain millions of records
- the original datasets are publicly available
- avoiding unnecessary duplication of NHS-hosted data

Users wishing to reproduce the analysis should download the source datasets directly from the official NHS websites listed above.

---

## Reproducibility

To reproduce the project:

1. Download English Prescribing Dataset files covering January 2025 to June 2026.
2. Filter the prescribing data to the selected GLP-1 chemical substances.
3. Standardise monthly schemas.
4. Append the files into a single prescribing dataset.
5. Build a presentation-level product classification.
6. Download June 2026 GP registered-population data.
7. Aggregate population to PracticeCode.
8. Download QOF 2025–26 diabetes data.
9. Retain practice-level Diabetes Register and Diabetes Prevalence fields.
10. Relate all datasets through the dimensional Power BI model described in the main project README.

For the complete methodology, analytical measures and interpretation, see the project documentation in:

`documentation/GLP1_Commercial_Analytics_Report.pdf`
