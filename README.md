# ANCOVA Examples (R and Python)

Short, practical examples of running analysis of covariance (ANCOVA) with assumption checks.

## What is ANCOVA?

ANCOVA compares group means on an outcome while statistically controlling for a continuous covariate (for example, a pre-test score or baseline measurement). This reduces noise and gives a fairer comparison between groups.

## Assumptions checklist

1. **Independence** of observations
2. **Linearity** between the covariate and the outcome in each group
3. **Homogeneity of regression slopes** (no group x covariate interaction)
4. **Approximately normal residuals**
5. **Homogeneity of variances** (Levene's test)

## R example

```r
library(car)
options(contrasts = c("contr.sum", "contr.poly"))

# 1. Check homogeneity of slopes (interaction should be non-significant)
m1 <- lm(score ~ group * pretest, data = df)
Anova(m1, type = 3)

# 2. Run the ANCOVA
m2 <- lm(score ~ group + pretest, data = df)
Anova(m2, type = 3)
```

## Python example

```python
import pandas as pd
import statsmodels.api as sm
from statsmodels.formula.api import ols

df = pd.read_csv("data.csv")

# 1. Check homogeneity of slopes
m1 = ols("score ~ C(group) * pretest", data=df).fit()
print(sm.stats.anova_lm(m1, typ=2))

# 2. Run the ANCOVA
m2 = ols("score ~ C(group) + pretest", data=df).fit()
print(sm.stats.anova_lm(m2, typ=2))
```

## Reporting your results

Report the F statistic with degrees of freedom, the p-value, an effect size (partial eta squared), and the **adjusted** (estimated marginal) means.

## Quick check

To quickly verify your results, you can use this [free online ANCOVA calculator](https://freestatisticaltools.com/ancova-calculator/).
