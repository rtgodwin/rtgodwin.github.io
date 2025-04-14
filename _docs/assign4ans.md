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
cps.mod <- lm(log(wage) ~ education + gender + age + experience + gender *
                education, data = cps)
summary(cps.mod)
```

```
CCoefficients:
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

Since it is a log-linear model, changes in the $X$ variables have _approximate_ $100\times\beta$ percentage change effects on `wage`. From the output above, workers with an education make 17.8% more compared to workers without an education.

### (c)

If there is heteroskedasticity (instead of homoskedasticity), then the standard errors, t-statistics, and p-values, are all wrong.

We can test for heteroskedasticity using White's test. The null hypothesis is that we have _homoskedasticity_, that is, $H_0:\operatorname{var}(\epsilon_i) = \sigma^2 \quad ; \quad \forall i$. The alternative hypothesis is heteroskedasticity. White's test requires we install a package, before performing the test:

```r
install.packages("skedastic")
library(skedastic)
white(cps.mod)
```

```
# A tibble: 1 × 5
  statistic p.value parameter method       alternative
      <dbl>   <dbl>     <dbl> <chr>        <chr>      
1      8.00   0.433         8 White's Test greater    
```
The output we are interested in is the p-value of 0.433. We fail to reject the null, and conclude that the error term is homoskedastic.

### (d)

Becuase the consequences of heteroskedasticity are severe (hypothesis tests are wrong), sometimes we ignore the results of the hypothesis test above. Hypothesis tests can be wrong! (Remember type II error.) We estimate the _heteroskedastic robust standard errors_ anyway.

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
