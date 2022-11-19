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

The three variables: `sinmom14yes`, `momed`, and `daded` all have small t-statistics which indicate they are all _individually_ insignificant. However, we know that dropping them all from the model, all at once, requires that all 3 variables be _jointly_ insignificant. To justify dropping these 3 variables from the model, we need to test the null hypothesis that:

$H_0: \beta_{sinmom14} = 0 \text{ and }$


Now, the 95% confidence interval is:

```r
3.848 - 1.977 * 0.356
```
```
## [1] 3.144188
```
```r
3.848 + 1.977 * 0.356
```
```
## [1] 4.551812
```

So, the interval is $[3.14, 4.55]$. This interval contains all values for any null hypotheses involving $\beta_1$ that we will fail to reject at the 5% significance level.

### Question 4
The hypothesis is formulated by:\
$H_0: \beta_1 = 4$\
$H_A: \beta_1 \neq 4$

The t-statistic for this hypothesis test is:
$t = \frac{b_1 - \beta_{1,0}}{s.e.(b_1)} = \frac{3.848 - 4}{0.356}$

Calculate this in R:

```r
tstat <- (3.848 - 4) / 0.356
tstat
```
```
## [1] -0.4269663
```
To get the p-value, we can use:

```r
2 * pt(tstat, df = 140, lower.tail = TRUE)
```
```
## [1] 0.6700598
```

Note that we want the area in the "tail" of the distribution, so in this case we had to use `lower.tail = TRUE`. We multiplied by 2 because $H_A$ is two sided.

With a p-value of 0.67 we fail to reject the null at the 10% significance level (or any common significance level).

### Question 5

Perhaps people consume more ice cream on the weekends. We can estimate the effect of `weekend` on `revenue` using the model:\

$revenue = \beta_0 + \beta_1weekend + \epsilon$

The R code to estimate the above population model is:

```r
summary(lm(revenue ~ weekend, data = mydata))
```
```
## 
## Call:
## lm(formula = revenue ~ weekend, data = mydata)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -30.032  -7.112  -0.649   7.891  46.030 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  501.315      1.217 411.791  < 2e-16 ***
## weekend       19.082      2.266   8.422 3.96e-14 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 12.23 on 140 degrees of freedom
## Multiple R-squared:  0.3363, Adjusted R-squared:  0.3315 
## F-statistic: 70.93 on 1 and 140 DF,  p-value: 3.958e-14
```

$b_0 = 501.32$ and $b_1 = 19.08$. $b_0$ is the sample average revenue on weekdays. $b_1$ is the difference between the sample average revenue on weekends compared to weekdays. That is, the sample average revenue is 19.08 higher on weekends. The sample average revenue on weekends is $b_0 + b_1 = 501.32 + 19.08 = 520.40$.

### Question 6

In other words, test the hypothesis that the revenue is the same on weekends as it is on weekdays. Although we estimated a difference of 19.08, this difference may not be significant given the variability of the estimator. The null hypothesis is $H_0: \beta_1 = 0$. This is the same as a **test of significance** of the dummy variable. R has already performed this test. The p-value is `3.96e-14`, which is very small. We reject the null hypothesis at any significance level. Weekends appear to make a difference for ice cream sales revenues.

### Question 7

Since `revenue` is the dependent variable, it should go on the y-axis. We can put the continuous variable `temp` on the x-axis, and we can graph the dummy variable `weekend` by giving each data point a different colour based on whether $weekend = 1$ or $weekend = 0$.

First we create this variable to determine the colour of each data point:

```r
mydata$mycol[mydata$weekend == 1] <- "orange"
mydata$mycol[mydata$weekend == 0] <- "purple"
```

Then we plot the data, add a legend, and add the estimated LS line to the plot:

```r
plot(x = mydata$temp, y = mydata$revenue,
     main = "Ice cream revenue and temperature",
     xlab = "Temperature",
     ylab = "Revenue",
     pch = 16,
     col = mydata$mycol)

legend("topright",
       legend = c("weekend", "weekday"),
       col=c("orange", "purple"), pch=16)

abline(lm(revenue ~ temp, data = mydata), col = "blue")
```
![](https://rtgodwin.com/3040/images/assign2addline-1.png)
