# LendingClub Loan Default Analysis

An exploratory data analysis of LendingClub loans, built in Python with pandas. The project works from a 5% random sample of the accepted loans (2007 to 2018 Q4) and asks a simple question: **do riskier loan grades actually default more often?**

## Key finding

Among loans with a final outcome (Fully Paid or Charged Off), the share that were charged off rises steadily as the grade gets riskier.

| Grade | Loans | Charged off |
|-------|-------|-------------|
| A | 11,822 | 5.9% |
| B | 19,655 | 13.3% |
| C | 19,084 | 22.6% |
| D | 10,045 | 30.4% |
| E | 4,639 | 39.1% |
| F | 1,587 | 48.3% |
| G | 446 | 48.2% |

- Overall, about **20%** of closed loans were charged off (13,468 of 67,278).
- Grade G has only 446 loans, so its rate is less reliable than the others. F and G land at nearly the same level.

## Data

- **Source:** the LendingClub accepted loans file (`accepted_2007_to_2018Q4.csv.gz`) from Kaggle. The full file is not included here because of its size.
- **Sample:** `LendingClub_sample.csv`, a 5% random sample (`random_state=42`) taken chunk by chunk, with 113,035 rows.
- **Columns kept (10):**

| Column | Description |
|--------|-------------|
| `loan_amnt` | Amount of the loan in USD |
| `term` | Loan length: 36 or 60 months |
| `int_rate` | Interest rate (%) |
| `grade` | LendingClub risk grade, A (safest) to G (riskiest) |
| `annual_inc` | Borrower's self-reported annual income |
| `purpose` | Reason for the loan (e.g. debt consolidation, credit card) |
| `addr_state` | Borrower's US state |
| `dti` | Debt-to-income ratio |
| `loan_status` | Current status of the loan |
| `issue_d` | Month the loan was issued |

## What the notebook does

`sample_data_set.ipynb` walks through these steps:

1. **Builds the sample.** Reads the large file in chunks of 200,000 rows, keeps 5% of each chunk, and saves the result as `LendingClub_sample.csv`.
2. **Gets an overview.** Checks shape, data types, summary statistics and missing values.
3. **Cleans the data.**
   - Finds and drops one row that is empty in every column.
   - Strips the leading spaces from `term` (` 36 months` becomes `36 months`).
   - Converts `issue_d` to a proper date (June 2007 to December 2018).
   - Leaves 89 missing `dti` values as they are for now.
4. **Frames Problem 1.** Keeps only loans with a final outcome (`Fully Paid` or `Charged Off`) and creates a `default` column (1 = charged off, 0 = fully paid). Loans that are still `Current` or in a grace period are excluded because their outcome isn't known yet.
5. **Calculates the default rate by grade** and plots it as a bar chart.

## Project status

- [x] Sampling, cleaning and date conversion
- [x] Problem 1: default rate by grade
- [ ] Problem 2: default rate by loan purpose (started, not finished)
- [ ] Further ideas: default by term, interest rate, DTI and income band, and a simple predictive model

## How to run

1. Install the requirements:

   ```bash
   pip install pandas matplotlib jupyter
   ```

2. Open `sample_data_set.ipynb`.
3. To skip the sampling step, load the ready-made sample instead of the full file:

   ```python
   import pandas as pd
   loans = pd.read_csv("LendingClub_sample.csv")
   ```

   Then continue from the cleaning step. The CSV still contains the all-empty row and the leading spaces in `term`, so those fixes need to be applied again.

4. To rebuild the sample from scratch, download the full file from Kaggle and change the `path` variable in the first code cell to point to it.

## Limitations

- The sample is 5% of the data, so small groups (such as grade G) have wide uncertainty.
- Only closed loans are used, which leaves out loans that are still active.
- This shows association, not cause. Grade is assigned by LendingClub using borrower information, so it already bundles many other factors.

## Tools

Python, pandas, matplotlib, Jupyter (VS Code).
