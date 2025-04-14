---
title: "Assignment 4 Answer Key"
permalink: /3040/assign4ans/
excerpt: "Polynomials, Logs, Interactions, Heteroskedasticity - Answer Key"
toc: false
---

------------------------------------------------------------------------

## Question 1

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

## Question 3

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

### (b)

The DiD estimate is the coefficient on the interaction term: 2.75.
