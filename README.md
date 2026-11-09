# Bank Customer Churn Prediction

**Type:** Classification

## Goal
Predict whether a bank customer will **leave the bank (1)** or **stay (0)**.

**Target:** `exited` (1 = customer left the bank, 0 = stayed)

## Data
The data is **synthetic** (fake, created with Python) so it can be big and have all the problems we want to practice on.

- **Generator:** `generate_data.py` (random seed = 123, so you always get the same data)
- **File:** `data/bank_customers.csv` - about 150,000 rows (+0.5% duplicate rows) and 52 columns
- **Data types in the file:** int, float, text (object) and dates saved as text

Re-create (or change) the data:
```bash
python generate_data.py
```
Change `N_ROWS` (100,000 - 300,000) or `SEED` at the top of `generate_data.py` to get a bigger / different dataset.

### Problems added on purpose
- **ID / personal columns** (IDs, names, e-mail, phone, date of birth)
- **Messy column names** (`Annual Income ($)`, `CreditScore`, `No. of Dependents`, ...)
- **Short codes** that should be full values (`M`/`F`, `Y`/`N`, `T`/`F`, `FT`/`PT`, ...)
- **Numbers saved as text** (`$1,250.00`, `12,500`, `34 yrs`, `12.5 km`, `15%`)
- **Missing values**, also hidden as text (`N/A`, `unknown`, `?`, `-`, a space) and one **mostly empty** column
- **Outliers** (extreme values) and **impossible values** (age = 150, negative mileage, 9999 codes, ...)
- **Dates saved as text** in different formats
- **Messy text** (`New York`, `NEW YORK`, `  new york`)
- **Categories saved as numbers** (`region_code`)
- **Useless columns**: a constant column, an almost-constant column, a duplicate column
- **Duplicate rows** (~0.5%)

### Columns
| Column in the file | Clean name | Problems |
|---|---|---|
| `Customer ID` | `customer_id` | ID / personal detail -> drop |
| `Full Name` | `full_name` | ID / personal detail -> drop |
| `E-mail` | `email` | ID / personal detail -> drop |
| `Phone #` | `phone_number` | ID / personal detail -> drop |
| `DOB` | `date_of_birth` | ID / personal detail -> drop |
| `Age (Years)` | `age` | number saved as text (`12 yrs`); 2% missing; wrong values: `-1`, `0`, `160` |
| `Gender` | `gender` | short codes: `M`, `F` |
| `Marital Status` | `marital_status` | short codes: `S`, `M`, `D`, `W` |
| `Education` | `education` | short codes: `HS`, `BSC`, `MSC`, `PHD` |
| `City` | `city` | 1% missing; mixed upper / lower case, extra spaces |
| `Region Code` | `region_code` | category saved as a number |
| `Preferred Language` | `preferred_language` | - |
| `Marketing Opt-In` | `marketing_opt_in` | short codes: `Y`, `N` |
| `Account No.` | `account_number` | ID / personal detail -> drop |
| `Account Open Date` | `account_open_date` | date saved as text (`%Y-%m-%d`) |
| `Last Transaction Date` | `last_transaction_date` | 2% missing; date saved as text (`%m-%d-%Y`) |
| `Tenure Years` | `tenure_years` | - |
| `CreditScore` | `credit_score` | 3% missing; wrong values: `0`, `999` |
| `Balance` | `balance` | number saved as text (`$1,250.00`); extreme outliers |
| `NumOfProducts` | `num_products` | - |
| `HasCrCard` | `has_credit_card` | short codes: `Y`, `N` |
| `IsActiveMember` | `is_active_member` | short codes: `Y`, `N` |
| `Estimated Salary ($)` | `estimated_salary` | number saved as text (`$1,250.00`); 4% missing; extreme outliers |
| `Monthly Transactions` | `monthly_transactions` | wrong values: `9,999` |
| `Avg Transaction Amount` | `avg_transaction_amount` | number saved as text (`$1,250.00`); 3% missing; extreme outliers |
| `Num Complaints 12m` | `num_complaints_12m` | - |
| `Complaint Resolved` | `complaint_resolved` | short codes: `T`, `F` |
| `Satisfaction Score` | `satisfaction_score` | 5% missing |
| `Card Type` | `card_type` | mixed upper / lower case, extra spaces |
| `Points Earned` | `points_earned` | - |
| `Mobile App User` | `mobile_app_user` | - |
| `Online Banking Logins Month` | `online_banking_logins_month` | - |
| `Num Branch Visits Month` | `num_branch_visits_month` | - |
| `Has Loan` | `has_loan` | - |
| `Loan Amount` | `loan_amount` | number saved as text (`1,250.00`) |
| `Has Mortgage` | `has_mortgage` | - |
| `Investment Account` | `investment_account` | - |
| `Overdraft Count 12m` | `overdraft_count_12m` | - |
| `Customer Segment` | `customer_segment` | 2% missing |
| `Contact Channel Preference` | `contact_channel_preference` | mixed upper / lower case, extra spaces |
| `Relationship Manager Assigned` | `relationship_manager_assigned` | - |
| `Last Campaign Response` | `last_campaign_response` | - |
| `Num Dependents` | `num_dependents` | 2% missing |
| `Employment Status` | `employment_status` | short codes: `EMP`, `SE`, `UNE`, `RET`, `STU` |
| `Fee Paid 12m` | `fee_paid_12m` | number saved as text (`$1,250.00`) |
| `Salary Deposit Flag` | `salary_deposit_flag` | - |
| `Referral Code` | `referral_code` | 80% missing (mostly empty) |
| `Bank Name` | `bank_name` | only one value |
| `Currency` | `currency` | only one value |
| `Is Employee` | `is_employee` | almost only one value |
| `Product Count` | `product_count` | exact copy of `num_products` |
| `Exited` | `exited` | **TARGET** (text: 'Yes', 'No') |

## Files
| File | Description |
|---|---|
| `generate_data.py` | Creates the dataset |
| `bank_customer_churn.ipynb` | The full project notebook |
| `data/bank_customers.csv` | The dataset |
| `data/bank_customers_metadata.xlsx` | Metadata (data dictionary): description, role, data types, missing values, statistics, codes and problems of every column |
| `models/` | Created when you run the notebook (best model, scaler, selected features) |

## Steps in the notebook
**A. Data cleaning**
- A0 Drop columns that are not useful (IDs, names, ...)
- A1 Rename columns
- A2 Data types: numbers saved as text, messy text, short codes, dates, categories saved as numbers, target to 0/1
- A3 Missing values - % per column, drop columns > 50% missing, fill the rest (mean / mode)
- A4 Outliers - box plots, IQR table, wrong values -> median, extreme values -> capping
- A5 Duplicate rows and duplicate columns
- A6 Columns with only one value

**B. EDA**
- B1 Target count and percentage
- B2 Correlation heatmap and correlation with the target
- B3 Charts for the independent features (and features vs target)

**C. Feature engineering**
- C1 Binning / new features
- C2 Label encoding and one-hot encoding
- C3 Data splitting: 80% train / 15% test / 5% unseen (random_state = 123)
- C4 Feature scaling (StandardScaler)
- C5 Feature selection (SelectKBest + Random Forest importance)
- C6 20,000-row training sample for comparing / tuning the models

**D. Feature extraction**
- D1 PCA
- D2 Factor Analysis
- D3 Cluster analysis (K-Means with elbow and silhouette)

**E. Machine learning (20 models)**
- E1 Define the models
- E2 Evaluation function (all metrics)
- E3 Cross validation (5 folds)
- E4 Training and testing evaluation
- E5 Hyperparameter dictionary
- E6 Grid search tuning
- E7 Compare the models, select the best, re-train it on the full training data
- E8 Save the best model (`joblib`)
- E9 Load the model
- E10 Predict and evaluate on the 5% unseen data

## How to run
```bash
pip install pandas numpy matplotlib seaborn scikit-learn xgboost joblib jupyter
python generate_data.py
```
Then open `bank_customer_churn.ipynb` **from this folder** and run all cells (the full notebook takes about 10-20 minutes).
