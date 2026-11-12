# Hospital Readmission Analysis: Patients with Diabetes (SQL + pandas)

## Executive Summary
This project analyzes about 100,000 hospital encounters of patients with diabetes to find which factors are associated with readmission within 30 days. Hospital 30-day readmission is a common hospital quality measure.

**Goal:** To identify which factors in the data are associated with 30-day readmission, and which are not.

**Headline finding:** The number of earlier inpatient admissions showed the clearest association: the 30-day rate rose from 8.6% with none to 36.3% with seven.

## Data

- **Source:** UCI Machine Learning Repository, "Diabetes 130-US Hospitals for Years 1999-2008" https://archive.ics.uci.edu/dataset/296/diabetes+130-us+hospitals+for+years+1999-2008. The citation and license URL: https://creativecommons.org/licenses/by/4.0/ and citation: Clore, J., Cios, K., DeShazo, J., & Strack, B. (2014). Diabetes 130-US Hospitals for Years 1999-2008. UCI Machine Learning Repository. https://doi.org/10.24432/C5230J
- **Size:** 101,766 rows and 50 columns.
- **What a row means:** each row is one hospital encounter (a stay of 1 to 14 days), not one patient. The encounters come from 69,990 unique patients, so some patients appear more than once.
- **Outcome column:** `readmitted` (`<30` = readmitted within 30 days, `>30` = after 30 days, `NO` = not readmitted).
- **Files:** `diabetic_data.csv` (main data) and `IDS_mapping.csv` (decodes admission type, discharge disposition, and admission source; three tables stacked in one file).
- **Missing values** are stored as `?`: weight 96.9%, max_glu_serum 94.7%, A1Cresult 83.3%, medical_specialty 49.1%, payer_code 39.6%, percentages are from all 101,766 rows before exclusion. For lab tests, missing most likely means the test was not measured.

## Project Structure

| File | Description |
|---|---|
| `diabetes_readmission_analysis.ipynb` | The full analysis |
| `diabetic_readmissions_data/diabetic_data.csv` | Main data (encounters) |
| `diabetic_readmissions_data/IDS_mapping.csv` | Lookup codes |
| `.gitignore` | Ignore Mac system, Python environment and cache files, etc |
| `README.md` | This file |


**Notebook sections**
1. **Load and clean the data:** check `diabetic_data.csv` (shape, columns, missing values), review `IDS_mapping.csv`, and exclude encounters that ended in death or hospice.
2. **Analysis:** examine the association of the 30-day readmission rate with age, length of hospital stay, earlier inpatient admissions, medical specialty, and HbA1c testing.
3. **Conclusions:** summarize which factors are strongly, somewhat, or hardly associated with readmission.

## Methods

- **Tools:** Python, pandas, and SQL (SQLite) in Google Colab. (The SQL `FILTER` clause needs SQLite 3.30 or later.)
- **30-day readmission rate:** the percentage of encounters with `readmitted = '<30'`.
- **Cleaning decision:** I excluded 2,423 encounters with discharge codes for death or hospice (11, 13, 14, 19, 20, 21), because those patients are not expected to be readmitted. The overall 30-day rate changed from 11.2% to 11.4%, and the conclusions did not change. All findings below use the 99,343 remaining encounters.
- **Small groups:** groups with very few encounters were excluded from comparisons (at least 100 encounters for earlier admissions, at least 500 for medical specialty), because their rates are unstable.

## Key Findings

| Factor | Association with 30-day readmission | Evidence |
|---|---|---|
| Earlier inpatient admissions | Clearest | 8.6% with none (66,245 encounters) rising to 13.3% with one (18,984), 17.9% with two (7,300), and 46.2% (higest) with eight (145 encounters); 43.1% with nine (109 encounters) is slightly lower. |
| Length of hospital stay | Some | Rising from 8.4% at one day to about 14.8% at ten days, then varying between 10.8% and 14.0% at 11 to 14 days. |
| Medical specialty | Some | Among 16 specialties with at least 500 encounters, rates ranged from 4.8% (Obstetrics and Gynecology) to 16.1% (Nephrology). About half of encounters have no specialty recorded. |
| Age | Little | Ages 30 and older ranged from 9.8% to 12.6%; ages 20 to 30 were highest (14.3%); under age 10 was lowest (1.9%) but small (160 encounters). |
| HbA1c test | Little | 9.9% with a test result vs 11.7% when not measured, a small difference. |

These are associations, not causes.

## Limitations

- Each row is an encounter, and some patients appear several times, so encounters are not fully independent.
- The data covers 1999 to 2008 and U.S. hospitals only, so patterns may differ today.
- Large amounts of data are missing (weight, lab tests, medical specialty), which limits what can be concluded.
- Groups with few encounters give unstable rates.
- This is a descriptive analysis. It shows associations but does not include statistical tests or predictive models.

## Ethical Considerations

-  I did not try to identify any patient in these datasets. The dataset is publicly available and de-identified and covers 130 U.S. hospitals.
- The results describe groups, not individuals, and must not be used to make decisions about an individual patient's care.
- This analysis is intended for **research and analytical purposes only**.

## How to Run

1. The two CSV files `diabetic_data.csv` and `IDS_mapping.csv` are included in the folder `diabetic_readmissions_data/` and are also available from the UCI page above (CC BY 4.0).
2. Open `diabetes_readmission_analysis.ipynb` in Google Colab, upload the two files, and update the file paths if needed.
3. Choose **Runtime, then Run all**.

## Author

Jason Lou 
Email: jasonlou622@gmail.com
LinkedIn: https://www.linkedin.com/in/jason-lou-53844a384
GitHub: https://github.com/JasonLou622