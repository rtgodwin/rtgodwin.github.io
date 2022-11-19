---
title: "Assignment 3 Answer Key"
permalink: /3040/assign3ans/
excerpt: "Schooling - Answer Key"
toc: false
---

------------------------------------------------------------------------

### Question 1

To estimate the model:

$wage = \beta_0 + \beta_1ed + \epsilon$

and view a summary of the results, we can use:

```r
summary(lm(wage ~ ed, data=school))
```
```
Coefficients:
            Estimate Std. Error t value Pr(>|t|)    
(Intercept)  216.803     36.364   5.962 2.98e-09 ***
ed            29.325      2.597  11.293  < 2e-16 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 256.5 on 1842 degrees of freedom
Multiple R-squared:  0.06475,	Adjusted R-squared:  0.06424 
F-statistic: 127.5 on 1 and 1842 DF,  p-value: < 2.2e-16
```
The estimated slope of 29.325 is interpreted as: it is estimated that an increase in education of 1 year leads to an increase in wage of 29 cents per hour.

### Question 2

The reason that we should include the other variable in the model is to avoid omitted variable bias (OVB). If a variable is correlated with education, and also determines wage, then leaving it out of the model will cause least-squares estimation to be biased. So, we need to estimate a "multiple regression model" that includes these other variables.

### Question 3

To estimate the model with wage on the LHS and all other variable on the RHS, we can use either:

```r
mod1 <- lm(wage ~ ., dat=school)
```
or
```r
mod1 <- lm(wage ~ ed + exp + iqscore + black + sinmom14 + momed + daded, data=school)
```

and then:
```r
summary(mod1)
```
```
Coefficients:
             Estimate Std. Error t value Pr(>|t|)    
(Intercept) -452.8176    64.3296  -7.039 2.72e-12 ***
ed            47.6204     3.3721  14.122  < 2e-16 ***
exp           28.1799     1.8580  15.167  < 2e-16 ***
iqscore        1.2859     0.4651   2.765  0.00575 ** 
blackyes     -81.2574    18.3346  -4.432 9.89e-06 ***
sinmom14yes   -7.6014    20.6584  -0.368  0.71295    
momed          2.7606     2.4434   1.130  0.25870    
daded          2.3158     2.2235   1.042  0.29777    
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 239.1 on 1836 degrees of freedom
Multiple R-squared:   0.19,	Adjusted R-squared:  0.1869 
F-statistic: 61.51 on 7 and 1836 DF,  p-value: < 2.2e-16
```

The model has become more accurate in terms of describing the wage data (the $R^2$ is now 0.19 instead of 0.07), but the main difference to pint out is the estimated returns to education. In this estimated model, the effect of education on wage is now 47.6 cents. This is 1.5 times the effect that was estimated in the model in question 1. The model in question 1 was suffering from omitted variable bias.

### Question 4

We can use the `predict()` function:

```r
predict(mod1, data.frame(ed=12, exp=5, iqscore=100, black="no", sinmom14="no",
                         momed=12, daded=12))
```
```
449.0337
```

In the code above, we have chosen a "representative" worker, and predicted their wage based on their characteristics. The model predicts that a worker with 12 years of education, 5 years work experience, an IQ of 100, who isn't black and whose mom wasn't single, and whose mom and dad each had 12 years of education, would make an hourly wage of 4.49 dollars per hour.

### Question 5

The three variables: `sinmom14yes`, `momed`, and `daded` all have small t-statistics which indicate they are all _individually_ insignificant. However, we know that dropping them all from the model, all at once, requires that all 3 variables be _jointly_ insignificant. To justify dropping these 3 variables from the model, we need to test the null hypothesis:

$H_0: \beta_{sinmom14} = 0 \text{ and } \beta_{momed} = 0 \text{ and } \beta_{daded} = 0$

We can do this by estimating the model under the null hypothesis (the "restricted" model, where we drop the 3 variables):

```r
mod2 <- lm(wage ~ ed + exp + iqscore + black, data=school)
```
and then calculating the F-statistic and p-value using the `anova()` function:

```r
anova(mod1, mod2)
```
```
Model 1: wage ~ ed + exp + iqscore + black + sinmom14 + momed + daded
Model 2: wage ~ ed + exp + iqscore + black
  Res.Df       RSS Df Sum of Sq     F Pr(>F)
1   1836 104992112                          
2   1839 105237952 -3   -245840 1.433 0.2313
```
With an F-stat of 1.433 and a p-value of 0.2313, we **fail to reject** the null hypothesis that all 3 variables are jointly insignificant. We can drop them from the model.

### Question 6

We have now estimated three different models, each providing a different estimate for the returns to education. The model from Question 1 likely suffers from omitted variable bias, so we shouldn't use this model. Using either the model from question 3 or from question 6, we could report the estimated returns to education as either 48 cents, or 49 cents. 
