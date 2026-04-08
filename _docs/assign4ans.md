---
title: "Assignment 4 Answer Key"
permalink: /3040/assign4ans/
excerpt: "Polynomials, Logs, Interactions, Heteroskedasticity - Answer Key"
toc: false
---

------------------------------------------------------------------------

## Question 1

### (a)

```r
did <- read.csv("https://rtgodwin.com/data/card.csv")
did.mod <- lm(EMP ~ STATE + TIME + STATE * TIME + CO_OWNED, data = did)
summary(did.mod)
```

```
Coefficients:
            Estimate Std. Error t value Pr(>|t|)    
(Intercept)  24.3025     1.1169  21.759  < 2e-16 ***
STATE        -2.9418     1.2141  -2.423 0.015625 *  
TIME         -2.2833     1.5403  -1.482 0.138637    
CO_OWNED     -2.6611     0.7141  -3.727 0.000208 ***
STATE:TIME    2.7500     1.7170   1.602 0.109659    
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 9.432 on 763 degrees of freedom
Multiple R-squared:  0.02533,	Adjusted R-squared:  0.02022 
F-statistic: 4.957 on 4 and 763 DF,  p-value: 0.0005991
```

The `CO_OWNED` variable helps control for differences in the _treatment_ and _control_ groups. The _parallel trends_ assumption says that the treatment and control groups must be identical (except for the minimum wage policy), but this is not realistic. Additional control variables, like `CO_OWNED`, help establish parallel trends.

### (b)

The DiD estimate is the coefficient on the interaction term: 2.75.

### (c)

In general, once we start adding additional control variables like `CO_OWNED`, we can't get the same estimate from the interaction term that we get using the table method. In this case however, we just happen to get 2.75 from both methods.

## Question 2

Load the data:

```r
cps <- read.csv("http://rtgodwin.com/data/cps1985.csv")
```

### (a)

Estimate the model using the `lm()` function:

```r
cps.mod <- lm(log(wage) ~ education + gender + age + experience, data = cps)
summary(cps.mod)
```

```
Coefficients:
            Estimate Std. Error t value Pr(>|t|)    
(Intercept)  0.89621    0.69229   1.295    0.196    
education    0.17746    0.11371   1.561    0.119    
gendermale   0.25736    0.03948   6.519 1.66e-10 ***
age         -0.07961    0.11365  -0.700    0.484    
experience   0.09234    0.11375   0.812    0.417    
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 0.4525 on 529 degrees of freedom
Multiple R-squared:  0.2703,	Adjusted R-squared:  0.2648 
F-statistic: 48.99 on 4 and 529 DF,  p-value: < 2.2e-16

```

### (b)

The estimated effect of education on wage has a percentage change interpretation, since wage is in logs. Each additional year of education is associated with an approximate 17.8% increase in wage. However, with a p-value of 0.119 the effect is not significant.

### (c)

If there is heteroskedasticity (instead of homoskedasticity), then the standard errors, t-statistics, and p-values, are all wrong.

We need to install two packages before we can estimate the "robust" standard errors (and associated t-statistics and p-values):

```r
install.packages("lmtest")
library(lmtest)
install.packages("sandwich")
library(sandwich)
coeftest(cps.mod, vcov = vcovHC(cps.mod, "HC1"))
```

```
t test of coefficients:

             Estimate Std. Error t value  Pr(>|t|)    
(Intercept)  0.896211   0.135704  6.6041 9.753e-11 ***
education    0.177461   0.011424 15.5345 < 2.2e-16 ***
gendermale   0.257361   0.039388  6.5340 1.507e-10 ***
age         -0.079611   0.011266 -7.0662 5.047e-12 ***
experience   0.092341   0.012371  7.4644 3.467e-13 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
```

There are some very important differences from the output in part (a). Under heteroskedasticity, all of the variables are now statistically significant!

