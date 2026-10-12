# Charitable Food Assistance: State-Level Regression Analysis

## Project Overview

This project studies variation in the number of households using charitable food assistance (CFA) across U.S. states.

Charitable food assistance is defined here as households receiving free groceries, free meals, or both through food banks, food pantries, soup kitchens, and other charitable food providers.

The analysis combines 2023 Current Population Survey Food Security Supplement estimates with USDA Economic Research Service data on population, poverty, educational attainment, and rurality.

The main goal is to understand which state characteristics are associated with higher levels of charitable food assistance use and to develop a framework that can later be extended to county-level estimates.

---

## Research Questions

The project focuses on several questions:

- How strongly is state population associated with the number of CFA households?
- Does poverty explain additional variation in CFA use across states?
- Are educational attainment and rurality associated with CFA participation after controlling for population and poverty?
- Are the results robust to influential observations and alternative model specifications?
- Can state-level relationships eventually be used to support county-level CFA projections?

---

## Data Sources

### 1. Current Population Survey Food Security Supplement

The CFA estimates are constructed from the 2023 CPS Food Security Supplement.

The main outcome is:

**CFA households**
= households receiving free groceries OR free meals.

Household survey weights are used to produce state-level estimates.

Replicate weights are used to estimate standard errors for CFA household counts.

---

### 2. USDA Economic Research Service

USDA ERS county-level datasets are used to construct state characteristics.

Variables include:

- 2023 state population
- 2023 poverty rate
- educational attainment
- county population
- Urban Influence Codes

Urban Influence Codes are also used to construct a measure of rural population.

For this project, counties with UIC codes **6, 8, or 9** are classified as rural.

The state rural measure is calculated as:

`population living in rural counties / total state population`

---

## Regression Specification

The dependent variable is:

`ln(CFA households)`

The main explanatory variables are:

- log state population
- state poverty rate
- percent of adults without a high school diploma
- percent of the state population living in rural counties

The models are estimated sequentially.

### Model 1

`ln(CFA households) = f(ln population)`

### Model 2

Adds the state poverty rate.

### Model 3

Adds educational attainment.

### Model 4

Adds rural population share.

Robust standard errors and sensitivity checks are also used to evaluate whether the main results depend heavily on a small number of influential states.

---

## Main Findings

The strongest relationship in the analysis is between state population and CFA household counts.

In the full specification, the coefficient on log population is approximately **1**, meaning that a 1% increase in state population is associated with approximately a 1% increase in the number of CFA households, holding the other variables constant.

The poverty rate is also positively and statistically significantly associated with CFA household counts.

A one percentage-point increase in the poverty rate is associated with approximately a 5% increase in CFA households in the full model.

Educational attainment and rural population share are not statistically significant in the baseline full specification.

The results remain broadly similar when:

- robust standard errors are used
- influential observations are excluded
- states with the largest studentized residuals are excluded

Overall, the analysis suggests that population size and poverty explain most of the observed cross-state variation in CFA household counts.

These results should be interpreted as descriptive associations rather than causal effects.

---

## Project Files

### Final Analysis Files

#### `cfa_state_regression_2023.dta`

Final state-level regression dataset.

This is the main analysis file and includes:

- CFA household estimates
- CFA standard errors
- CFA household share
- state population
- poverty rate
- educational attainment
- rural population share
- transformed regression variables

This file is used for the final regression analysis.

---

#### `cfa_state_regression_2023.log`

Stata log containing the regression workflow and output.

Includes:

- descriptive statistics
- baseline regression models
- robust standard errors
- variance inflation factors
- heteroskedasticity tests
- residual diagnostics
- influence statistics
- sensitivity checks

---

### CFA Estimate Files

#### `cfa_state_household_counts_2023_wide.dta`

State-level CFA estimates in wide format.

Contains separate state-level estimates for:

- free groceries
- free meals
- households receiving both
- households receiving either form of assistance

This is the main CFA input used to construct the regression dataset.

---

#### `cfa_state_household_counts_2023_long.dta`

Same CFA estimates stored in long format.

Useful for:

- tables
- visualization
- comparing CFA categories across states

---

#### `cfa_state_household_counts_2023_long_with_us.dta`

Long-format CFA estimates including a national U.S. observation.

Useful when comparing individual states with national estimates.

---

#### `cfa_state_household_counts_2023_wide_with_us.dta`

Wide-format CFA estimates including the national U.S. observation.

---

#### `cfa_state_base_2023.dta`

Reduced CFA state-level file used as the base for merging external explanatory variables.

Contains the main outcome variables and state identifiers before USDA ERS data are merged.

---

### USDA ERS Files

#### `ers_population_state_2023.dta`

State-level 2023 population estimates.

Used to construct:

`ln_population_2023`

Population is the primary scale variable in the regression models.

---

#### `ers_county_population_2023.dta`

County-level 2023 population estimates.

Used to calculate the share of each state's population living in rural counties.

---

#### `ers_poverty_state_2023.dta`

State-level 2023 poverty rates.

Used to test whether economic disadvantage is associated with greater CFA use.

---

#### `ers_education_state_2023.dta`

State-level educational attainment data.

The main measure is the percentage of adults who are not high school graduates.

---

#### `ers_uic_2024.dta`

County-level 2024 Urban Influence Codes.

Used to classify counties by rurality.

Counties with UIC codes 6, 8, or 9 are classified as rural for this analysis.

---

#### `ers_rural_state_2023.dta`

State-level rural population measure constructed from county population estimates and Urban Influence Codes.

The main variable is:

`rural_pop_share_pct`

which represents the percentage of a state's population living in counties classified as rural.

---

## Data Construction Workflow

The analysis follows the general sequence below:

1. Construct state-level CFA household estimates from the 2023 CPS Food Security Supplement.
2. Extract state population estimates from USDA ERS data.
3. Extract state poverty rates.
4. Extract state educational attainment measures.
5. Merge county population data with Urban Influence Codes.
6. Identify rural counties using UIC codes 6, 8, and 9.
7. Calculate the share of each state's population living in rural counties.
8. Merge all variables into a single 51-observation state-level dataset.
9. Create log-transformed variables.
10. Estimate regression models and diagnostic tests.

The final sample contains all 50 states and the District of Columbia.

---

## Model Diagnostics

The project also includes several diagnostic checks.

### Multicollinearity

Variance Inflation Factors are used to check for multicollinearity among the explanatory variables.

The mean VIF is approximately 2.35, suggesting that severe multicollinearity is not present.

### Heteroskedasticity

Both the Breusch-Pagan and White tests fail to reject the null hypothesis of constant variance.

Robust standard errors are nevertheless estimated as an additional check.

### Influential Observations

Cook's distance, leverage, and studentized residuals are used to identify potentially influential observations.

Sensitivity regressions excluding these states produce similar coefficients for population and poverty, suggesting that the main findings are not driven by a small number of states.

---

## Ongoing Extensions

The current analysis is being extended in several directions.

Planned additions include:

- testing nonlinear effects of population
- adding state population density
- testing nonlinear effects of population density
- adding the share of the population under age 18
- adding the share of the population age 65 and older
- replacing poverty with log median household income as a robustness check

A longer-term goal is to use these state-level relationships to help construct county-level CFA estimates.

---

## Why This Project Matters

Administrative or survey-based estimates of charitable food assistance are not always readily available at detailed geographic levels.

Understanding how CFA participation varies with population, poverty, demographics, and geography can help support:

- food assistance planning
- geographic targeting
- food bank and pantry capacity planning
- identification of areas with potentially high demand
- development of county-level estimates where direct survey estimates are unavailable

The project is intended as a descriptive and predictive exercise, not a causal analysis.

---

## Software

The analysis is conducted primarily in:

- Stata
- CPS Food Security Supplement microdata
- USDA ERS county-level datasets

---

## Author

Radha Vagheswara Subramanian  
M.S. Applied Economics  
University of Maryland, College Park
