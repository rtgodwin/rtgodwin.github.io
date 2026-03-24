---
title: "Assignment 3 Answer Key"
permalink: /3040/assign3ans/
excerpt: "Schooling - Answer Key"
toc: false
---

------------------------------------------------------------------------

<!--
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
-->

## Question 1
### Part (a)
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

<!--
The model has become more accurate in terms of describing the wage data (the $R^2$ is now 0.19 instead of 0.07), but the main difference to point out is the estimated returns to education. In this estimated model, the effect of education on wage is now 47.6 cents. This is 1.5 times the effect that was estimated in the model in question 1. The model in question 1 was suffering from omitted variable bias.
-->

### Part (b)

The three variables: `sinmom14yes`, `momed`, and `daded` all have small t-statistics which indicate they are all _individually_ insignificant. However, we know that dropping them all from the model, all at once, requires that all 3 variables be _jointly_ insignificant. To justify dropping these 3 variables from the model, we need to use an F-test and test the null hypothesis:

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

You don't have to use the `anova()` function for this question, you could also calculate the F-stat "by hand" and compare it to the critical value of 2.60.

## Question 2

Download the data:

```r
diam <- read.csv("https://rtgodwin.com/data/diamond.csv")
```

### (a)

```r
mod1 <- lm(price ~ carat + I(carat^2) + colour + clarity, data = diam)
summary(mod1)
```

```
Coefficients:
            Estimate Std. Error t value Pr(>|t|)    
(Intercept)   2028.6      224.2   9.049  < 2e-16 ***
carat         4351.3      687.6   6.328 9.16e-10 ***
I(carat^2)    6455.1      522.5  12.353  < 2e-16 ***
colourE      -1261.1      169.3  -7.448 1.04e-12 ***
colourF      -1679.2      158.9 -10.566  < 2e-16 ***
colourG      -2046.8      162.8 -12.574  < 2e-16 ***
colourH      -2543.0      164.8 -15.434  < 2e-16 ***
colourI      -3225.5      173.0 -18.646  < 2e-16 ***
clarityVS1   -1168.6      121.0  -9.661  < 2e-16 ***
clarityVS2   -1561.8      131.5 -11.875  < 2e-16 ***
clarityVVS1   -287.1      130.3  -2.203   0.0284 *  
clarityVVS2   -836.9      120.8  -6.927 2.69e-11 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 577.3 on 296 degrees of freedom
Multiple R-squared:  0.9723,	Adjusted R-squared:  0.9712 
F-statistic:   943 on 11 and 296 DF,  p-value: < 2.2e-16
```

`colour` and `clarity` are categorical variables - R has automatically created a _system_ of dummy variables for us.

### (b)

The effect of a 0.1 increase in `carat` will have a **non-constant** effect on `price` (that is the appeal of the polynomial model). In a polynomial model, we can interpret the estimated results by considering specific scenarios. For example, let's get the predicted increase in `price` due to an increase in `carat` of 0.1, when the diamond has a size of 0.1:

```r
predict(mod1, data.frame(carat = 0.2, colour = "D", clarity = "VS2")) - predict(
  mod1, data.frame(carat = 0.1, colour = "D", clarity = "VS2"))
```

```
628.7867
```

This has taken the difference in the predicted price of a diamond of size `carat = 0.2` and `carat = 0.1`. The predicted effect is \$628.79.

When using the `predict` function, we must choose values for all of the $X$ variables. I arbitrarily chose `colour = "D"` and `clarity = "VS2"`. As we will see in the next part, these choices do not matter for the predicted effect.

The whole point of the non-linear model is that the effect of `carat` on `price` is non-constant. To illustrate that you understand this, you need to get the predicted effect of an increase of carats of 0.1, _for a different starting value_. For example, I will compare the effect of a 0.1 increase for when `carat = 0.5`:

```r
predict(mod1, data.frame(carat = 0.6, colour = "D", clarity = "VS2")) - predict(
  mod1, data.frame(carat = 0.5, colour = "D", clarity = "VS2"))
```

```
1145.196
```

The same increase in size of 0.1 has almost double the effect when the diamond is 0.5 carats vs. 0.1 carats.

## Question 3

### Part (a)

```r
cps <- read.csv("http://rtgodwin.com/data/cps1985.csv")
mod <- lm(log(wage) ~ education + gender + age + experience, data = cps)
summary(mod)
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

### Part (b)
The estimated coefficient of 0.17746 is interpreted as: an increase in education of 1 year is associated with an increase in wages of 17.8% on average.

<!--
### Question 6

We can use the `predict()` function:

```r
predict(mod2, data.frame(ed=12, exp=5, iqscore=100, black="no"))
```
```
440.1436
```

In the code above, we have chosen a "representative" worker, and predicted their wage based on their characteristics. The model predicts that a worker with 12 years of education, 5 years work experience, an IQ of 100, and who isn't black, would make an hourly wage of 4.40 dollars per hour.

You don't have to use the `predict()` function for this question, you could also calculate a predicted value "by hand".
-->
