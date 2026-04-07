---
title: "Lab 5"
permalink: /3040/lab6/
excerpt: "Fixed effects estimation - Manitoba trade data"
toc: false
---

------------------------------------------------------------------------

## Download the trade data

Download the data from the website using:

```r
trade <- read.csv("https://rtgodwin.com/data/trade2.csv")
```

## Pooled LS

Ignore the panel structure and estimate the model:

```r
mod <- lm(log(exports) ~ log(gdp) + log(distance), data = trade)
summary(mod)
```

```
Coefficients:
              Estimate Std. Error t value Pr(>|t|)    
(Intercept)    5.74166    0.41817   13.73   <2e-16 ***
log(gdp)       0.83199    0.01115   74.63   <2e-16 ***
log(distance) -1.15155    0.04733  -24.33   <2e-16 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 0.2753 on 189 degrees of freedom
Multiple R-squared:  0.9794,	Adjusted R-squared:  0.9792 
F-statistic:  4499 on 2 and 189 DF,  p-value: < 2.2e-16
```
A 1% increase in GDP of the importing province/territory is associated with a 0.83% increase in Manitoba's exports to that location. For every 1% increase in distance, exports decline by 1.15%.

## Fixed effects estimation (manual)

```r
fe.dummies <- lm(log(exports) ~ log(gdp) + partner - 1, data = trade)
summary(fe.dummies)
```
Here, we manually put in the dummy variable `partner' (the variable takes on 1 of 12 provinces/territories as possible trading partners). R creates 12 dummies. The `-1` gets rid of the intercept $\bets_0$ in order to avoid the dummy variable trap.

```
Coefficients:
              Estimate Std. Error t value Pr(>|t|)    
(Intercept)    5.74166    0.41817   13.73   <2e-16 ***
log(gdp)       0.83199    0.01115   74.63   <2e-16 ***
log(distance) -1.15155    0.04733  -24.33   <2e-16 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 0.2753 on 189 degrees of freedom
Multiple R-squared:  0.9794,	Adjusted R-squared:  0.9792 
F-statistic:  4499 on 2 and 189 DF,  p-value: < 2.2e-16
```

## IV estimation using `ivreg()`

To estimate the model:

$$wage = \beta_0 + \beta_1education + \beta_2urban + \beta_3gender + \beta_4ethnicity + \beta_5unemp + \epsilon$$

using IV estimation, and where _distance_ is an instrument for _education_, we can use:

```r
install.packages("ivreg")
library(ivreg)
iv <- ivreg(wage ~ education + urban + gender + ethnicity + unemp |
    distance + urban + gender + ethnicity + unemp, data = college)
summary(iv)
```

```
Coefficients:
                  Estimate Std. Error t value Pr(>|t|)    
(Intercept)       -0.65702    1.83641  -0.358   0.7205    
education          0.64710    0.13594   4.760 1.99e-06 ***
urbanyes           0.04614    0.06039   0.764   0.4449    
gendermale         0.07075    0.04997   1.416   0.1569    
ethnicityhispanic -0.12405    0.08871  -1.398   0.1621    
ethnicityother     0.22724    0.09863   2.304   0.0213 *  
unemp              0.13916    0.00912  15.259  < 2e-16 ***
```

## IV estimation using 2SLS approach

In the first stage we get the LS predicted values from a regression of the endogenous variable _education_ on the instrument, and all other $x$ variables:

```r
first.stage <- lm(education ~ urban + gender + ethnicity + unemp + distance,
    data = college)
education.hat <- first.stage$fitted.values
```

In the second stage we estimate the original population model, but we replace the variable _distance_ with the predicted values from the first stage:

```r
iv <- lm(wage ~ education.hat + urban + gender + ethnicity + unemp,
data = college)
summary(iv)
```

The results are the same as from the `ivreg()` package!
