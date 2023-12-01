---
title: "Lab 5"
permalink: /3040/lab5/
excerpt: "Interaction terms and instrumental variables"
toc: false
---

------------------------------------------------------------------------

# Interaction terms

## Download the CPS data set {#cps}

Download the data from the website using:

```r
cps <- read.csv("https://rtgodwin.com/data/cps1985.csv")
```

## Estimate a model with an interaction term

```r
model1 <- lm(log(wage) ~ education + gender + education*gender + experience + region  
             + occupation + union, data = cps)
summary(model1)
```

```
Coefficients:
                     Estimate Std. Error t value Pr(>|t|)    
(Intercept)           0.85222    0.20568   4.143 3.99e-05 ***
education             0.08417    0.01329   6.330 5.27e-10 ***
gendermale            0.53085    0.19880   2.670 0.007815 ** 
experience            0.01066    0.00166   6.420 3.07e-10 ***
regionsouth          -0.10630    0.04159  -2.556 0.010876 *  
occupationoffice     -0.21713    0.07616  -2.851 0.004531 ** 
occupationsales      -0.34956    0.09161  -3.816 0.000152 ***
occupationservices   -0.40104    0.08073  -4.968 9.18e-07 ***
occupationtechnical  -0.04780    0.07290  -0.656 0.512373    
occupationworker     -0.21475    0.07605  -2.824 0.004927 ** 
unionyes              0.20197    0.05099   3.960 8.52e-05 ***
education:gendermale -0.02449    0.01475  -1.661 0.097386 .  
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 0.4293 on 522 degrees of freedom
Multiple R-squared:  0.3519,	Adjusted R-squared:  0.3382 
F-statistic: 25.77 on 11 and 522 DF,  p-value: < 2.2e-16
```
The interaction term allows for the effect of education on wage to differ between men and women. For example, the effect of an additional year of education is to increase wagges by 8.4% for women, and to increase wages by (0.08417 - 0.02449 = 0.05968) 6.0% for men.

To test to see if there is a difference in the effect of education between men and women, we need to test the significance of the interaction term (it's what's allowing for there to be a difference!). If there is no difference, then the effect of the interaction is 0. The null hypothesis is:

$H_0:$ effect of education on wage is the same for men and women

or equivalently

$H_0: \beta_{education \times gendermale} = 0$

R has already tested this hypothesis for us. The p-value is 0.097386. It is borderline; we fail to reject the null hypothesis at the 5% level, suggesting there are no differences in the effects of education on wage, between men and women.

# Instrumental variables estimation

## Download the Card (1993) wage data

```r
college <- read.csv("https://rtgodwin.com/data/collegedist.csv")
```

The variables in the data are:
  - _wage_ - the dependent $y$ variable
  - _education_ - number of years of education of the worker
  - _distance_ - distance from the nearest college. This will be our instrument $z$.
  - Other demographic variables used as controls.

## IV estimation using `ivreg()`

To estimate the model:

$$wage = \beta_0 + \beta_1education + \beta_2urban + \beta_3gender + \beta_4ethnicity + \beta_5unemp + \epsilon$$


