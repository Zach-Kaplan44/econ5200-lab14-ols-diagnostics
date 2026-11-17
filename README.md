# econ5200-lab14-ols-diagnostics
# OLS Diagnostics: Robust and Clustered Standard Errors

## Objective

I tested how well classical, heteroskedasticity-robust, and cluster-robust standard errors hold up for OLS slope inference when the usual error assumptions fail, and checked an AI-generated standard error table against statsmodels.

## Methodology

- Ran a simulation that fits OLS repeatedly and records how often a nominal 95% confidence interval contains the true slope.
- Generated data with heteroskedastic errors and compared coverage under classical standard errors against coverage under HC3.
- Generated data with errors shared within 40 firms and compared coverage under HC3 against coverage under standard errors clustered by firm.
- Planted three unusual rows in a dataset and tried to recover them using leverage, studentised residuals, and Cook's distance.
- Refit the model with and without the flagged rows to see how much each one moved the estimated slope.
- Wrote my own prompt asking an AI to produce a table of standard errors, then checked every number it returned against statsmodels output.
- Iterated over two rounds, diagnosing the cause of the mismatches in round 1 before re-prompting.
- Repeated the round 2 check on a copy of the data with every column renamed, to see whether the result depended on the column names.

## Key Findings

**Coverage.** With heteroskedastic errors, classical standard errors gave 92.4% coverage and HC3 gave 94.4%, so HC3 came close to the nominal 95% while classical standard errors were a little too narrow. With errors shared within 40 firms, HC3 gave only 50.3% coverage, while clustering by firm gave 93.7%. Robust standard errors fix heteroskedasticity but not within-group correlation; that requires clustering at the level where the correlation lives.

**Influential observations.** Leverage, studentised residuals, and Cook's distance together recovered all three planted rows. One of them was enough on its own to move the estimated slope from 1.4927 to 1.4468.

**AI-generated table.** In round 1, 0 of 18 numbers matched statsmodels, because the function the AI wrote rounded each standard error to 5 decimal places. After I revised the prompt, round 2 matched all 18 exactly. Running the same check on a copy of the data with every column renamed also gave 18 of 18, so the match did not depend on the column names.
