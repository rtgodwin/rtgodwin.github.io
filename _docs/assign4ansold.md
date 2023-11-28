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
mod1 <- lm(price ~ carat + I(carat^2) + I(carat^3) + colour + clarity, data = diam)
summary(mod1)
```

```
Coefficients:
            Estimate Std. Error t value Pr(>|t|)    
(Intercept)   1553.8      452.8   3.431 0.000686 ***
carat         7590.7     2770.9   2.739 0.006530 ** 
I(carat^2)     669.0     4823.2   0.139 0.889771    
I(carat^3)    3067.1     2541.7   1.207 0.228502    
colourE      -1279.3      169.9  -7.532 6.16e-13 ***
colourF      -1695.7      159.4 -10.638  < 2e-16 ***
colourG      -2058.0      162.9 -12.632  < 2e-16 ***
colourH      -2558.6      165.1 -15.493  < 2e-16 ***
colourI      -3251.9      174.2 -18.664  < 2e-16 ***
clarityVS1   -1199.8      123.6  -9.707  < 2e-16 ***
clarityVS2   -1585.1      132.8 -11.934  < 2e-16 ***
clarityVVS1   -308.7      131.4  -2.349 0.019493 *  
clarityVVS2   -863.4      122.7  -7.036 1.38e-11 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 576.8 on 295 degrees of freedom
Multiple R-squared:  0.9724,	Adjusted R-squared:  0.9713 
F-statistic: 865.9 on 12 and 295 DF,  p-value: < 2.2e-16
```

`colour` and `clarity` are categorical variables - R has automatically created a _system_ of dummy variables for us.

### (b)

The model estimated above has a polynomial with a cubed term in it, so that the degree is 3 ($r = 3$). From the summary output above, we see that the variable $carat^3$ is _insignificant_ (the p-value is 0.228502). That is, we fail to reject to the null hypothesis that the effect of $carat^3$ is zero - we can drop this variable from the model. We now estimate a model with $r=2$:

$$price = \beta_0 + \beta_1carat + \beta_2carat^2 + ...$$

To estimate this model in R, we can use:

```r
mod2 <- lm(price ~ carat + I(carat^2) + colour + clarity, data = diam)
summary(mod2)
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

We now see that the the highest order term in the polynomial ($carat^2$) is now significant (p-value is approximately 0). We stop estimating models and testing: the degree of the polynomial is 2 ($r=2$).

### (c)

The effect of a 0.1 increase in `carat` will have a **non-constant** effect on `price` (that is the appeal of the polynomial model). In a polynomial model, we can interpret the estimated results by considering specific scenarios. For example, let's get the predicted increase in `price` due to an increase in `carat` of 0.1, when the diamond has a size of 0.1:

```r
predict(mod2, data.frame(carat = 0.2, colour = "D", clarity = "VS2")) - predict(
  mod2, data.frame(carat = 0.1, colour = "D", clarity = "VS2"))
```

```
628.7867
```

This has taken the difference in the predicted price of a diamond of size `carat = 0.2` and `carat = 0.1`. The predicted effect is \$628.79.

When using the `predict` function, we must choose values for all of the $X$ variables. I arbitrarily chose `colour = "D"` and `clarity = "VS2"`. As we will see in the next part, these choices do not matter for the predicted effect.

The whole point of the non-linear model is that the effect of `carat` on `price` is non-constant. To illustrate that you understand this, you need to get the predicted effect of an increase of carats of 0.1, _for a different starting value_. For example, I will compare the effect of a 0.1 increase for when `carat = 0.5`:

```r
predict(mod2, data.frame(carat = 0.6, colour = "D", clarity = "VS2")) - predict(
  mod2, data.frame(carat = 0.5, colour = "D", clarity = "VS2"))
```

```
1145.196
```

The same increase in size of 0.1 has almost double the effect when the diamond is 0.5 carats vs. 0.1 carats.

### (d)

Because we are taking the _difference_ between two predicted values, it does not matter what values we choose for the other $X$ variables that are not changing (their effects cancel out). To illustrate this, I will choose different values for `colour` and `clarity`, but the same increase from `carat = 0.1` to `carat = 0.2`:

```r
predict(mod2, data.frame(carat = 0.2, colour = "E", clarity = "VS1")) - predict(
  mod2, data.frame(carat = 0.1, colour = "E", clarity = "VS1"))
```

```
628.7867
```

The predicted effect of an increase of 0.1 carats, for when `carat = 0.1`, has not changed compared to above. In fact, since the effects of the other variables (and the intercept) cancel out since they are on both sides of the $-$ sign, we do not need to consider them in our calculation:

$$\hat{price}|_{carat = 0.2} - \hat{price}|_{carat=0.1} = 4351.3(0.2) + 6455.1(0.2^2) - 4351.3(0.1) - 6455.1(0.1^2) = 628.78$$

## Question 2

Load the data:

```r
cps <- read.csv("http://rtgodwin.com/data/cps1985.csv")
```

### (a)

We can include the interaction term by including `gender * education` in the `lm()` function:

```r
cps.mod <- lm(log(wage) ~ education + gender + age + experience + gender *
                education, data = cps)
summary(cps.mod)
```

```
Coefficients:
                     Estimate Std. Error t value Pr(>|t|)    
(Intercept)           0.53764    0.70887   0.758 0.448521    
education             0.18311    0.11333   1.616 0.106753    
gendermale            0.69499    0.20315   3.421 0.000672 ***
age                  -0.06472    0.11345  -0.570 0.568616    
experience            0.07754    0.11355   0.683 0.494959    
education:gendermale -0.03362    0.01531  -2.196 0.028545 *  
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 0.4509 on 528 degrees of freedom
Multiple R-squared:  0.2769,	Adjusted R-squared:  0.2701 
F-statistic: 40.44 on 5 and 528 DF,  p-value: < 2.2e-16
```

### (b)

Since it is a log-linear model, changes in the $X$ variables have _approximate_ $100\times\beta$ percentage change effects on `wage`. From the output above, women with an education make 18.3% more compared to women without an education. The effect for men is different, due to the _interaction_ term in the model. Men with an education make $0.18311 - 0.03362 = 15.0%$ more than men without an education. The interaction term, `education:gendermale` allows for an "adjustment" to be made to the effect for when the dummy variables `education` and `gendermale` both equal 1.

### (c)

Since the interaction term is significant at the 5% level, we reject the null that there is no difference between the return to education for men and women.

### (d)

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
1      7.37   0.690        10 White's Test greater    
```
The output we are interested in is the p-value of 0.690. We fail to reject the null, and conclude that the error term is homoskedastic.

### (e)

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
(Intercept)           0.537643   0.194521  2.7639 0.0059104 ** 
education             0.183114   0.011411 16.0471 < 2.2e-16 ***
gendermale            0.694988   0.191017  3.6384 0.0003013 ***
age                  -0.064716   0.013117 -4.9339 1.082e-06 ***
experience            0.077542   0.014099  5.4997 5.936e-08 ***
education:gendermale -0.033616   0.014731 -2.2819 0.0228902 *  
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
```

There are some very important differences from the output in part (a). Under heteroskedasticity, all of the variables are now statistically significant!
