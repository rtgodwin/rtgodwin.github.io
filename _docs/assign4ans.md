---
title: "Assignment 4 Answer Key"
permalink: /3040/assign4ans/
excerpt: "Polynomials, Logs, Interactions - Answer Key"
toc: false
---

------------------------------------------------------------------------

### Question 1

We need to estimate a model with all variables, and squared and cubed terms. To get the squared and cubed terms, we can include them in the `lm()` function directly using `I()` (`I(age ^ 2)` for example). We can also create the squared and cubed terms beforehand:

```r
cps$age2 <- cps$age ^ 2
cps$age3 <- cps$age ^ 3
cps$yrseduc2 <- cps$yrseduc ^ 2
cps$yrseduc3 <- cps$yrseduc ^ 3
```

and then estimate the model:

```r
mod1 <- lm(ahe ~ female + age + age2 + age3 + yrseduc + yrseduc2 + yrseduc3 + location, data=cps)
summary(mod1)
```

```
Coefficients:
                    Estimate Std. Error t value Pr(>|t|)    
(Intercept)       -5.487e+00  2.536e+00  -2.164   0.0305 *  
female            -4.200e+00  7.062e-02 -59.467  < 2e-16 ***
age                1.764e+00  1.350e-01  13.066  < 2e-16 ***
age2              -2.966e-02  3.335e-03  -8.893  < 2e-16 ***
age3               1.578e-04  2.646e-05   5.962 2.50e-09 ***
yrseduc           -5.132e+00  4.381e-01 -11.714  < 2e-16 ***
yrseduc2           4.961e-01  3.473e-02  14.286  < 2e-16 ***
yrseduc3          -1.155e-02  8.892e-04 -12.992  < 2e-16 ***
locationnortheast  1.227e+00  1.050e-01  11.679  < 2e-16 ***
locationsouth      5.335e-03  9.454e-02   0.056   0.9550    
locationwest       7.346e-01  1.001e-01   7.340 2.15e-13 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 8.656 on 61384 degrees of freedom
Multiple R-squared:  0.2696,	Adjusted R-squared:  0.2695 
F-statistic:  2266 on 10 and 61384 DF,  p-value: < 2.2e-16
```

To determine the appropriate degree of the polynomials, we start with one of the variables (`age` for example). We test the significance of the highest degree polynomial: `age3`. If this variable is _insignificant_, we drop it from the model and re-estimate. Then, we repeat the process, starting by testing the significance of `age2`. If we ever find that the variable is insignificant, we stop the process, and do not drop it from the model. The same process applies for the `yrseduc` variables as well.

Since `age3` is significant, we leave it in the model. The order of the polynomial in "age" is 3. Since `yrseduc3` is significant, we leave it in the model. The order of the polynomial in "years of education" is also 3.

### Question 2

We can include the interaction terms by including `female*age`, for example, in the `lm()` function:

```r
mod2 <- lm(ahe ~ female + age + age2 + age3 + yrseduc + yrseduc2 + yrseduc3 
           + female*age + female*age2 + female*age3 
           + female*yrseduc + female*yrseduc2 + female*yrseduc3 
           + location, data=cps)
summary(mod2)
```

```
Coefficients:
                    Estimate Std. Error t value Pr(>|t|)    
(Intercept)       -9.476e+00  3.258e+00  -2.908  0.00363 ** 
female             5.327e+00  5.234e+00   1.018  0.30876    
age                2.077e+00  1.794e-01  11.579  < 2e-16 ***
age2              -3.468e-02  4.434e-03  -7.821 5.32e-15 ***
age3               1.822e-04  3.520e-05   5.176 2.27e-07 ***
yrseduc           -5.792e+00  5.474e-01 -10.582  < 2e-16 ***
yrseduc2           5.616e-01  4.368e-02  12.855  < 2e-16 ***
yrseduc3          -1.350e-02  1.122e-03 -12.032  < 2e-16 ***
locationnortheast  1.218e+00  1.049e-01  11.615  < 2e-16 ***
locationsouth      1.197e-02  9.442e-02   0.127  0.89912    
locationwest       7.416e-01  9.995e-02   7.419 1.19e-13 ***
female:age        -7.454e-01  2.719e-01  -2.741  0.00613 ** 
female:age2        1.216e-02  6.717e-03   1.811  0.07017 .  
female:age3       -6.091e-05  5.329e-05  -1.143  0.25299    
female:yrseduc     1.672e+00  9.192e-01   1.820  0.06884 .  
female:yrseduc2   -1.713e-01  7.241e-02  -2.365  0.01802 *  
female:yrseduc3    5.160e-03  1.847e-03   2.793  0.00522 ** 
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 8.644 on 61378 degrees of freedom
Multiple R-squared:  0.2717,	Adjusted R-squared:  0.2715 
F-statistic:  1431 on 16 and 61378 DF,  p-value: < 2.2e-16
```

### Question 3

We need to take the difference between the predicted `wage` for when education equals 13 years, and for when education equals 12 years:

```r
w13 <- predict(mod2, data.frame(female = 0,age = 40, age2 = 40^2, age3 = 40^3, 
                                yrseduc = 13, yrseduc2 = 13^2, yrseduc3 = 13^3,
                                location = "south"))
w12 <- predict(mod2, data.frame(female = 0,age = 40, age2 = 40^2, age3 = 40^3,
                                yrseduc = 12, yrseduc2 = 12^2, yrseduc3 = 12^3, 
                                     location = "south"))
w13 - w12
```

```
1.913595
```

This says that wage is predicted to increase by 1.91 dollars per hour, for an additional year of education, when the worker already has 12 years of education.

We had to provide values for all of the variables, because we are calculating an LS predicted value. The values for `female`, `age`, and `location` do not affect the predicted difference of `1.913595`, because the values of those variables are the same for both predicted values, so when we subtract them, they cancel out.

The predicted effect of wage on education of `1.913595` depends on the value of education that we started at (12). To see this, we can try the same process above, but starting at 16 years of education:

```r
w17 <- predict(mod2, data.frame(female = 0,age = 40, age2 = 40^2, age3 = 40^3, 
                                yrseduc = 17, yrseduc2 = 17^2, yrseduc3 = 17^3,
                                location = "south"))
w16 <- predict(mod2, data.frame(female = 0,age = 40, age2 = 40^2, age3 = 40^3,
                                yrseduc = 16, yrseduc2 = 16^2, yrseduc3 = 16^3, 
                                location = "south"))
w17 - w16
```

```
1.707046
```

The effect of education on wage is _diminishing_.

### Question 4

Download the data:

```r
cps <- read.csv("http://rtgodwin.com/data/cps1985.csv")
```

Estimate the model assuming homoskedasticity:

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

If there is heteroskedasticity (instead of homoskedasticity), then the standard errors, t-values, and p-values, are all wrong. We can test for heteroskedasticity using:

```r
install.packages("skedastic")
library(skedastic)
white(cps.mod)
```

```
statistic p.value parameter method       alternative
      <dbl>   <dbl>     <dbl> <chr>        <chr>      
1      7.37   0.690        10 White's Test greater    
```

The null hypothesis is that we have _homoskedasticity_, that is, $H_0:\operatorname{var}(\epsilon_i) = \sigma^2 \quad ; \quad \forall i$. With a p-value of 0.69, we fail to reject the null, and conclude that there is no heteroskedasticity.

Becuase the consequences of heteroskedasticity are severe (hypothesis tests are wrong), sometimes we ignore the results of the hypothesis test above. Hypothesis tests can be wrong! (Remember type II error.) We estimate the _heteroskedastic robust standard errors_ anyway:

```r
install.packages("sandwich")
library(sandwich)
install.packages("lmtest")
library(lmtest)
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

There are some very big and important differences. Under heteroskedasticity, all of the variables are now statistically significant!
