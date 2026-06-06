# Lab 9: Time Series Analyses -- Complete Final Exam Study Guide

**Source:** Lucas 2013 paper on casino poker room profitability
**Dataset:** `casino_poker_data.csv` -- 217 days of data (Feb-Sept 2009) from 3 Las Vegas casino resorts
**Covers:** Lectures 17-18 (Time Series + Critique of model selection -> inference workflow)

---

## THE BIG PICTURE (What This Lab Is Really About)

Lab 9 is NOT just about time series. It's about the **correct statistical inference workflow** vs. the **incorrect model selection -> inference workflow**. The lab uses the Lucas 2013 paper as a case study of what NOT to do, and walks you through what SHOULD have been done instead.

**Central critique:** The author performed variable selection (removing covariates, adding AR terms, adding dummy variables for outlier dates) and THEN used the resulting "final model" for inference. This is the model selection -> inference problem we learned about in Lecture 13.

---

## PART 1: Understanding the Lucas 2013 Paper

### Q1.1 -- Goal of the Paper
**Answer: 4** -- To determine the extent to which poker rooms drive traffic to slot machines and table games at a casino.

Key concept: The paper isn't asking whether poker rooms alone are profitable. It's asking whether having a poker room brings customers who ALSO play slots and table games (a "loss leader" argument).

### Q1.2 -- Shortcoming of the Paper
**Answer: 2** -- The author's workflow inappropriately mixes model selection with statistical inference.

This is THE key concept of the entire lab. The author:
- Performed variable selection (backward/forward selection guided by p-values)
- Used the resulting model for inference (hypothesis tests on coefficients)
- This is circular -- you can't use p-values to build a model and then trust the p-values from that model

### Q1.3 -- Double-Log Transformation Justification
**Answer: 2** -- The primary reason for variable transformation should be to improve the linear fit, NOT for "ease of interpretation."

Key distinction:
- Bad reason to transform: "it makes coefficients interpretable as elasticities"
- Good reason to transform: "examining the data shows that log-transforming improves the linear relationship between X and Y"

---

## PART 2: Understanding the Data

### Dataset Structure
The dataset has columns:
- `date`, `day_num`, `dow` (day of week)
- Event indicator variables: `PRESDAY`, `STPATS`, `KDERBY`, `MEMDAY`, `INDDAY`, `LABORDAY`, `NCAABBALL`
- `TREND` (natural log of day_num)
- Resort 1: `R1_COIN`, `R1_DROP`, `R1_RAKE`
- Resort 2: `R2_COIN`, `R2_DROP`, `R2_RAKE`
- Resort 3: `R3_COIN`, `R3_DROP`, `R3_RAKE`

### Q2.1 -- Variable Descriptions
**Answer: c(15, 13, 4, 11, 9)**

| Variable | Choice | Description |
|----------|--------|-------------|
| `dow` | 15 | Day of the week |
| `NCAABBALL` | 13 | Indicator for NCAA Basketball Championship games |
| `R1_COIN` | 4 | Total money wagered by customers on slot machines in Resort 1 |
| `R2_RAKE` | 11 | Money earned by the poker room in Resort 2 |
| `R3_DROP` | 9 | Mysterious measure of business volume on table games in Resort 3 |

**Know these variable meanings:**
- **COIN-IN (COIN):** Total money customers put into slot machines (NOT profit -- casino keeps ~7.5%)
- **DROP:** Measure of business volume on table games (casino keeps ~15% as profit)
- **RAKE:** Amount the casino poker room earned from poker tables (this IS the actual profit)

### Q2.2 -- Primary Predictor Variables
**Answer: c(10, 11, 12)** -- The RAKE variables (poker room earnings) for all three resorts.

The scientific question is: does poker room activity (RAKE) predict/drive slot machine and table game activity (COIN-IN and DROP)?

---

## PART 3: The CORRECT Statistical Inference Workflow

### The Pre-specified Model for Inference

This is the model the lab says we SHOULD use:

```
COIN_pf = beta_0 + beta_1*RAKE + beta_2*TUE + beta_3*WED + beta_4*THU + beta_5*FRI + beta_6*SAT + beta_7*SUN
```

Key features:
- Variables on raw (dollar) scale, NOT log-transformed
- Day of week included as a confounder (weekends are busier for everything)
- Monday as baseline level for day of week
- NO variable selection -- model is determined BEFORE looking at results
- NO AR/MA terms
- NO dummy variables for outlier dates

### Q3.1 -- Why This Model Differs from Lucas 2013
**Answer: c(3, 5)**

- **Choice 3 is correct:** The final models in Lucas 2013 were obtained by performing variable selection, and this is objectively incorrect to do (before inference).
- **Choice 5 is correct:** It would be difficult to scientifically justify why each of the six models should be different from each other. If the scientific question is the same (does RAKE predict COIN/DROP?), the model structure should be the same across resorts.

Why the others are wrong:
- Choice 1: It's not *objectively* correct to omit AR/MA terms -- you could argue for them a priori
- Choice 2: True about log transforms, but that's about model selection not this specific question
- Choice 4: Day of week is NOT the "only possible" confounding variable

### Q3.2 -- Summary Tables

**Q3.2.1: Summary table on original dollar scale**

R code pattern:
```r
summary_usd <- matrix(NA, nrow=9, ncol=4)
rownames(summary_usd) <- c("R1 COIN-IN", "R1 DROP", "R1 RAKE",
                            "R2 COIN-IN", "R2 DROP", "R2 RAKE",
                            "R3 COIN-IN", "R3 DROP", "R3 RAKE")
colnames(summary_usd) <- c("Mean", "Std. Dev", "Min.", "Max.")

# Fill in for each variable, e.g.:
summary_usd["R1 COIN-IN",] <- c(mean(casino$R1_COIN), sd(casino$R1_COIN),
                                  min(casino$R1_COIN), max(casino$R1_COIN))
# ... repeat for all 9 rows
```

**Q3.2.2: Profit-adjusted summary table**

Key profit margins:
- **COIN-IN profit = COIN-IN * 0.075** (casino keeps 7.5% of slot machine wagering)
- **DROP profit = DROP * 0.15** (casino keeps 15% of table game drop)
- **RAKE is already profit** (no adjustment needed)

```r
casino$R1_COIN_pf <- casino$R1_COIN * 0.075
casino$R1_DROP_pf <- casino$R1_DROP * 0.15
# RAKE variables stay as-is
# Repeat for R2 and R3
```

### Q3.3 -- Scatterplot Observations
**Answer: c(3, 4)**

- **Choice 3:** Visual inspection shows ~$2500 increase in RAKE is associated with ~$25,000 increase in slot profit
- **Choice 4:** Some slight concern about heteroskedasticity (variance increases with RAKE)

```r
ggplot(casino, aes(x=R1_RAKE, y=R1_COIN_pf)) + geom_point()
```

### Q3.4 -- Primary Analysis

**Q3.4.1: Running the regression**
```r
casino$dow <- relevel(as.factor(casino$dow), ref="Monday")
model1 <- lm(R1_COIN_pf ~ R1_RAKE + dow, data=casino)
summary(model1)

primary_coef <- coef(model1)["R1_RAKE"]     # the RAKE coefficient
pval <- summary(model1)$coefficients["R1_RAKE", 4]  # p-value for RAKE
```

**Q3.4.2: Interpreting the output**
**Answer: c(2, 4)**

- **Choice 2 is correct:** Every $1 increase in poker room profit is associated with an expected $2.449 increase in slot machine profit.
- **Choice 4 is correct:** Saturdays are the busiest day for slots (largest positive day-of-week coefficient).

Why others are wrong:
- Choice 1: The intercept ($132,669.654) is NOT the estimated profit on a Monday -- it's the estimated profit when RAKE=0 AND it's Monday
- Choice 3: You should NOT remove non-significant day-of-week dummies. Day of week was pre-specified as a confounder; you keep all levels.

**CRITICAL EXAM CONCEPT:** Do NOT remove non-significant covariates from a pre-specified inference model. That would be doing variable selection -> inference.

### Q3.5 -- Model Diagnostics (Checking Conditions)

This section tests ALL FOUR conditions for valid linear regression inference:

**Q3.5.1: Linearity Condition**
**Answer: 2** (likely NOT violated)

Plot: residuals vs. fitted values (or residuals vs. RAKE)
```r
casino$residuals <- residuals(model1)
casino$fitted <- fitted(model1)
ggplot(casino, aes(x=fitted, y=residuals)) + geom_point()
```

**Q3.5.2: Independence Condition (temporal trend)**
**Answer: 1** (MAYBE violated)

Plot: residuals vs. time (day_num)
```r
ggplot(casino, aes(x=day_num, y=residuals)) + geom_point()
```
Shows some cyclical patterns, but adjusting for day-of-week helped remove some of it.

**Q3.5.3: Normality Condition**
**Answer: 0** (LIKELY VIOLATED)

Three checks:
```r
# Histogram
hist(casino$residuals)

# QQ-plot
qqnorm(casino$residuals)
qqline(casino$residuals)

# Shapiro-Wilk test
st <- shapiro.test(casino$residuals)
```
The Shapiro-Wilk test likely rejects normality (p < 0.05).

**Q3.5.4: Equal Variance (Homoskedasticity) Condition**
**Answer: 1** (MAYBE violated)

Plot residuals vs. each predictor variable:
```r
ggplot(casino, aes(x=R1_RAKE, y=residuals)) + geom_point()
# Also plot residuals vs. each day of week (boxplots)
```

**Summary of diagnostics:**
| Condition | Status | Code |
|-----------|--------|------|
| Linearity | Likely NOT violated | 2 |
| Independence | MAYBE violated | 1 |
| Normality | LIKELY VIOLATED | 0 |
| Equal Variance | MAYBE violated | 1 |

---

## PART 4: Sensitivity Analyses + Critique of Lucas 2013

### Q4.1 -- Log-Scale Summary Table
**Key point:** You cannot just take log of the summary_usd matrix! You must compute `mean(log(x))`, not `log(mean(x))`.

```r
# WRONG: log(summary_usd)
# RIGHT: compute summary stats on log-transformed variables
summary_log["R1 COIN-IN",] <- c(mean(log(casino$R1_COIN)), sd(log(casino$R1_COIN)),
                                  min(log(casino$R1_COIN)), max(log(casino$R1_COIN)))
```

This is a common mistake! `log(mean(x)) != mean(log(x))` in general.

### Q4.2 -- Double-Log Scatterplot Observations
**Answer: c(3, 5)**

- **Choice 3:** Slope looks ~0.5, meaning a 1% increase in RAKE is associated with ~0.5% increase in COIN-IN
- **Choice 5:** A reasonable justification for log-transforming would be if PREVIOUS STUDIES showed improved model fit (a priori justification)

Why others are wrong:
- Choice 1: The original scale didn't really violate linearity, so the log doesn't "fix" anything
- Choice 2: Heteroskedasticity concerns were mild on the original scale
- Choice 4: Using YOUR OWN data to decide on transformation IS model selection (not allowed before inference)

**EXAM TRAP:** Choice 4 vs Choice 5. Using your own data to decide = model selection (bad). Using previous studies to decide = a priori justification (acceptable).

### Q4.3 -- Time Series Plot
**Answer: 2** -- The plot shows a cyclical pattern across time, similar to Figure 3 in Lucas 2013.

```r
ggplot(casino, aes(x=day_num, y=log(R1_COIN))) + geom_line()
```

### Q4.4 -- AR(1) Model Simulation

**Q4.4.1: Simulating AR(1) data**
```r
set.seed(1)
y <- numeric(217)
y[1] <- 14.2
for (i in 2:217) {
  y[i] <- y[i-1] + rnorm(1)  # c=0, phi_1=1, epsilon ~ N(0,1)
}
day <- 1:217
AR_df <- data.frame(day=day, y=y)
ggplot(data=AR_df, aes(x=day, y=y)) + geom_line()
```

The AR(1) formula: y_t = c + phi_1 * y_{t-1} + epsilon_t
With c=0, phi_1=1: this is a **random walk**.

**Q4.4.2: Comparing AR(1) simulation to actual data**
**Answer: c(3, 5)**

- **Choice 3:** The simulated AR(1) data drift much further from 14.2 than the actual log(COIN) values. This is because a random walk (phi_1=1) has no mean-reversion.
- **Choice 5:** The actual log(COIN) data show cyclical patterns explainable by day of week (weekday vs weekend cycles).

Why others are wrong:
- Choice 1: They do NOT look remarkably similar
- Choice 2: Visual similarity should NOT be used to justify including AR terms in an inference model
- Choice 4: Random walk cycles are NOT explained by day of week

### Q4.5 -- The "Final Model" from Lucas 2013

**Q4.5.1: Critique of the final model**
**Answer: c(1, 4, 6)**

- **Choice 1:** The log transformation at least appears to be determined a priori (before the analysis), which is correct.
- **Choice 4:** Day of week is categorical with 7 levels. In ANY variable selection, you should not remove individual days -- either keep all or remove all. Removing Thursday alone is wrong.
- **Choice 6:** Using correlograms to decide whether to include AR(1) terms IS model selection that should not be done before inference.

Why others are wrong:
- Choice 2: The constant/intercept 13.8990 is on the LOG scale, so the actual value would be e^13.8990, not $13.8990
- Choice 3: In a double-log model, the coefficient 0.0646 means a 1% increase in RAKE is associated with a 0.0646% increase in COIN (not 6.46%)
- Choice 5: Using data to identify outlier dates and then adding dummy variables for them IS model selection (not appropriate before inference)

**Q4.5.2: Running the ARIMA model**

```r
# Design matrix with all covariates from the "final model"
X <- cbind(
  log(casino$R1_RAKE),
  as.numeric(casino$dow == "Tuesday"),
  as.numeric(casino$dow == "Wednesday"),
  as.numeric(casino$dow == "Friday"),
  as.numeric(casino$dow == "Saturday"),
  as.numeric(casino$dow == "Sunday"),
  casino$STPATS,
  casino$MEMDAY,
  casino$INDDAY,
  as.numeric(casino$day_num == which(casino$date == "2009-03-12")),
  as.numeric(casino$day_num == which(casino$date == "2009-06-19"))
)
colnames(X) <- c("RAKE", "TUE", "WED", "FRI", "SAT", "SUN",
                  "STPATS", "MEMDAY", "INDDAY", "MAR12", "JUN19")

ar1_arima <- Arima(log(casino$R1_COIN), xreg=X, order=c(1,0,0))
coeftest(ar1_arima)
```

Key R functions:
- `Arima()` from the `forecast` package (note capital A)
- `order=c(1,0,0)` means AR(1), no differencing, no MA
- `xreg=X` passes the design matrix of covariates
- `coeftest()` from the `lmtest` package displays coefficient table

---

## KEY CONCEPTS FOR THE FINAL EXAM

### 1. The Model Selection -> Inference Problem
- You CANNOT use data to select your model and then use that same model for inference
- P-values from a model that was built via variable selection are INVALID
- The model for inference must be pre-specified BEFORE looking at results
- This applies to: variable selection, adding/removing AR/MA terms based on correlograms, adding outlier dummy variables based on residual analysis

### 2. Correct Inference Workflow
1. Pre-specify the model based on scientific rationale (BEFORE analysis)
2. Make summary tables and graphical summaries
3. Run the primary analysis
4. Check model diagnostics (linearity, independence, normality, equal variance)
5. Perform sensitivity analyses

### 3. Four Conditions for Linear Regression Inference
| Condition | How to Check | Plot |
|-----------|-------------|------|
| Linearity | Residuals vs. fitted values | Should show no pattern |
| Independence | Residuals vs. time | Should show no temporal pattern |
| Normality | Histogram, QQ-plot, Shapiro-Wilk | Should be bell-shaped, on the line, p > 0.05 |
| Equal Variance | Residuals vs. each predictor | Should show constant spread |

### 4. Log Transformations
- Double-log model: log(Y) = beta_0 + beta_1 * log(X) -- coefficients are elasticities
- In double-log: a 1% increase in X is associated with a beta_1% increase in Y
- Justification must be a priori (from previous studies or scientific reasoning), NOT from examining your own data
- log(mean(x)) != mean(log(x)) -- compute stats AFTER transforming, not before

### 5. AR(1) Models
- Formula: y_t = c + phi_1 * y_{t-1} + epsilon_t
- When phi_1 = 1 and c = 0: this is a random walk (non-stationary, drifts)
- Decision to include AR terms based on correlograms = model selection = invalid before inference

### 6. Categorical Variables in Regression
- A categorical variable with k levels needs k-1 indicator (dummy) variables
- Use `relevel()` to set the baseline category
- Do NOT selectively remove individual levels from a categorical variable
- If day of week is a confounder, keep ALL days even if some aren't significant

### 7. Key R Code Patterns
```r
# Set baseline level for factor
casino$dow <- relevel(as.factor(casino$dow), ref="Monday")

# Linear regression
model <- lm(Y ~ X + dow, data=casino)
summary(model)

# Extract coefficient and p-value
coef(model)["X"]
summary(model)$coefficients["X", 4]

# Residual diagnostics
residuals(model)
fitted(model)

# Shapiro-Wilk test for normality
shapiro.test(residuals(model))

# Summary statistics
mean(), sd(), min(), max()

# ARIMA with covariates
library(forecast)
Arima(y, xreg=X, order=c(1,0,0))

# Display ARIMA coefficients
library(lmtest)
coeftest(arima_model)
```

### 8. Interpreting Regression Output
- Intercept: estimated Y when ALL predictors = 0 (and baseline category for factors)
- Coefficient for continuous variable: expected change in Y for a 1-unit increase in X, holding other variables constant
- Coefficient for factor level: expected difference from baseline level, holding other variables constant
- P-value < 0.05 does NOT mean you should keep the variable; p-value > 0.05 does NOT mean you should remove it (in a pre-specified model)

---

## COMMON MISTAKES AND TRICKY PARTS

1. **Confusing COIN-IN with profit.** COIN-IN is total wagered, not profit. Casino keeps ~7.5%.
2. **Taking log of summary stats instead of computing summary stats of log values.** log(mean(x)) != mean(log(x)).
3. **Thinking non-significant p-values mean you should remove variables.** In a pre-specified model, you keep everything.
4. **Thinking the "Constant" in a log model is interpretable in dollars.** e^13.8990, not $13.8990.
5. **Confusing double-log coefficient interpretation.** beta_1 = 0.0646 means a 1% increase in X yields a 0.0646% increase in Y, NOT a 6.46% increase.
6. **Thinking using YOUR data to justify transformation is ok.** Only a priori justification (previous studies) is acceptable for pre-specifying transforms.
7. **Removing Thursday from day-of-week.** Categorical variables are all-or-nothing in variable selection.
8. **Using visual similarity to justify AR terms.** Just because data "look like" an AR process doesn't justify including AR terms in an inference model.

---

## ANSWER KEY (Quick Reference)

| Question | Answer | Key Concept |
|----------|--------|-------------|
| Q1.1 | 4 | Paper's goal: poker drives traffic to slots/tables |
| Q1.2 | 2 | Model selection -> inference is the shortcoming |
| Q1.3 | 2 | Transform to improve fit, not for interpretation |
| Q2.1 | c(15,13,4,11,9) | Variable definitions |
| Q2.2 | c(10,11,12) | Primary predictors are RAKE variables |
| Q3.1 | c(3,5) | Variable selection is wrong; models shouldn't differ |
| Q3.3 | c(3,4) | Positive relationship with mild heteroskedasticity |
| Q3.4.2 | c(2,4) | Coefficient interpretation; Saturdays busiest |
| Q3.5.1 | 2 | Linearity: likely NOT violated |
| Q3.5.2 | 1 | Independence: MAYBE violated |
| Q3.5.3 | 0 | Normality: LIKELY violated |
| Q3.5.4 | 1 | Equal variance: MAYBE violated |
| Q4.2 | c(3,5) | Log slope ~0.5; use prior studies to justify transform |
| Q4.3 | 2 | Time plot shows cyclical pattern |
| Q4.4.2 | c(3,5) | AR(1) drifts more; real data has day-of-week cycles |
| Q4.5.1 | c(1,4,6) | Log a priori OK; don't split categoricals; AR from correlograms = model selection |
