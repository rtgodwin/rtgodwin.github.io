---
title: "Lab 4"
permalink: /3040/lab4/
excerpt: "Polynomials, logs, interaction terms, and the F-test"
toc: false
---

------------------------------------------------------------------------

# Download the data

This lab uses life expectancy and health care expenditure data from Greene, example 6.10 (Greene, W. (2012). H.(2012): Econometric analysis. Journal of Boston: Pearson Education, 803-806). Download the data from the website using:

```r
health <- read.csv("https://rtgodwin.com/data/health.csv")
```

The data is from 1997, and contains information on 191 countries with:

| Variable                   	| Description 	|
|------------------------------	|--------	|
| DALE                  	| Disability adjusted life expectancy, in years    	|
| HEXP 	| Per capita health care expenditure    	|
| HC3                   	| Education   	|
| OECD  | A dummy variable =1 if country is in OECD, 0 otherwise |
| GINI | The GINI coefficient of income inequality |
| GEFF | An index measuring government effectiveness |
| VOICE | An index measuring the level of democracy |
| TROPICS | A dummy variable =1 if country is in the tropics, 0 otherwise |
| POPDEN | A measure of population density |
| PUBTHE | Proportion of health expenditure paid by bublic authorities |
| GDPC | GDP per capita |

In this lab, we want to investigate whether OECD countries are more efficient in their delivery of health care (DALE), through spending and education. That is, we want to see if the effect of HEXP and HC3 on DALE is different for OECD vs. non-OECD countries.

Before we can do this, we need to specify the right _functional form_. We need to see if there are non-linearities between the variables of interest.

Create a plot of life expectancy and education:

```r
plot(health$HC3, health$DALE)
```

![](https://rtgodwin.com/3040/images/p1.png)

There appears to be a possible non-linear (concave) relationship between education and life expectancy. To capture this, I will use a polynomial of HC3. Next, let's look at a plot of life expectancy and health spending:

```r
plot(health$HEXP, health$DALE)
```

![](https://rtgodwin.com/3040/images/p2.png)

There is definitely a non-linear relationship here. It is possible that, instead of a dollar amount change in spending, a proportional (percentage) change in spending has a constant effect of life expectancy. To capture this idea, we take the log of spending:

```r
plot(log(health$HEXP), health$DALE)
```

![](https://rtgodwin.com/3040/images/p3.png)

We have "linearized" the relationship! Before we start modelling, let's colour-code the data point by OECD status:

```r
health$col <- "red"
health$col[health$OECD == 1] <- "blue"
plot(log(health$HEXP), health$DALE, 
     main = "Health care expenditure and life expectancy by OECD status",
     xlab = "log health expenditure", ylab = "disability adjusted life expectancy",
     pch = 16, cex = 1, col = health$col)
legend("bottomright", c("non OECD", "OECD"), pch = 16, col = c("red", "blue"))
```

![](https://rtgodwin.com/3040/images/p4.png)

From the plot, it is difficult to tell whether the effect of spending on life expectancy differs between OECD and non-OECD countries.

In order to allow the effect of education and spending to differ by OECD status, we need to allow the OECD dummy to _interact_ with the education and spending. We also need to allow for the non-linear relationship between the variables. To accomplish all this, we'll estimate the model:

$$DALE = \beta_0 + \beta_1 \log(HEXP) + \beta_2 HC3 + \beta_3HC3^2 + \beta_4HC3^3 + \beta_5OECD + GINI + TROPICS + POPDEN + PUBTHE$$

$$+ GDPC + VOICE + GEFF + OECD \times log(HEXP) + OECD \times HC3 + OECD \times HC3sq$$

```
##    wage education experience age ethnicity region gender occupation
## 1  5.10         8         21  35  hispanic  other female     worker
## 2  4.95         9         42  57      cauc  other female     worker
## 3  6.67        12          1  19      cauc  other   male     worker
## 4  4.00        12          4  22      cauc  other   male     worker
## 5  7.50        12         17  35      cauc  other   male     worker
## 6 13.07        13          9  28      cauc  other   male     worker
##          sector union married
## 1 manufacturing    no     yes
## 2 manufacturing    no     yes
## 3 manufacturing    no      no
## 4         other    no      no
## 5         other    no     yes
## 6         other   yes      no
```

Notice that many of the variables do not contain numbers, but are instead characters (words). For example, the `ethnicity` variable takes on values "hispanic", "cauc", and "other". In order to use variables such as `ethnicity`, `region` and `gender`, we need to create dummy variables from them. From the `ethnicity` variable for example, we would create 2 dummies, even though there are 3 categories (in order to avoid the dummy variable trap).

R will recognize that variables like `ethnicity` contain several categories, and will automatically create the system of dummies for us. To see this, try:

```r
summary(lm(wage ~ ethnicity, data = cps))
```

```
## Call:
## lm(formula = wage ~ ethnicity, data = cps)
## 
## Residuals:
##    Min     1Q Median     3Q    Max 
## -8.278 -3.765 -1.278  2.205 35.222 
## 
## Coefficients:
##                   Estimate Std. Error t value Pr(>|t|)    
## (Intercept)         9.2779     0.2439  38.032   <2e-16 ***
## ethnicityhispanic  -1.9946     1.0146  -1.966   0.0498 *  
## ethnicityother     -1.2196     0.6711  -1.817   0.0697 .  
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 5.117 on 531 degrees of freedom
## Multiple R-squared:  0.01227,    Adjusted R-squared:  0.008545 
## F-statistic: 3.297 on 2 and 531 DF,  p-value: 0.03776
```
How would you interpret the estimated coefficients?

Answer: Hispanics make 1.9946 less on average compared to whites, and other ethnicities make 1.2196 less on average compared to whites, according to this data.

# Estimate the returns to education

Let's try some models in order to estimate the returns to education. Start with a simple model:

$wage = \beta_0 + \beta_1 education + \beta_2 gender + \epsilon$

Estimate this population model, and view the results, using:

```r
model1 <- lm(wage ~ education + gender, data = cps)
summary(model1)
```

```
## Call:
## lm(formula = wage ~ education + gender, data = cps)
## 
## Residuals:
##    Min     1Q Median     3Q    Max 
## -8.888 -2.997 -0.709  2.255 35.888 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept) -1.90623    1.04354  -1.827   0.0683 .  
## education    0.75128    0.07682   9.779  < 2e-16 ***
## gendermale   2.12406    0.40283   5.273 1.96e-07 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 4.639 on 531 degrees of freedom
## Multiple R-squared:  0.1884, Adjusted R-squared:  0.1853 
## F-statistic: 61.62 on 2 and 531 DF,  p-value: < 2.2e-16
```

What are the estimated returns to education?

Answer: An additional year of education is associated with an additional \$0.75 in wages.

# LS predicted values for a representative case {#rep}

One use of the estimated model above (`model1`), is to created an OLS predicted value for a "representative case". That is, we could use the model to predict the wage of an individual with certain characteristics. How much (according to the estimated model) can a woman with 12 years of education expect to make? We can get this number with the following code:

```r
predict(model1, data.frame(education = 12, gender = "female"))
```

```
##        1 
## 7.109175
```

This is the same as $-1.906 + 0.751(12) + 2.124(0) = 7.106$.

We just "plug" the numbers into the estimated equation. Take a moment to let the usefulness of prediction wash over you. You want to know how much a house will sell for? How much product you will sell at a certain price? How much GDP will be lost if the temperature rises by 3$^{\circ}$C?

# Avoiding OVB - estimate a bigger model {#OVB}

In order to avoid omitted variable bias, we need to include variables that are correlated with `education`, and are determinants of `wage`. Notice that things like `age` and `experience` are correlated with `education` (think about why):

```r
cor(cps$education, cps$experience)
```
```
## [1] -0.3526764
```
```r
cor(cps$education, cps$age)
```
```
## [1] -0.1500195
```

Hence, when we add these variables to the regression, we should expect the estimate for the returns to education to change. Estimate a model using all of the variables available in the data set:

```r
model2 <- lm(wage ~ education + experience + age + ethnicity + region + gender 
             + occupation + sector + union + married, data = cps)
summary(model2)
```

```
## Call:
## lm(formula = wage ~ education + experience + age + ethnicity + 
##     region + gender + occupation + sector + union + married, 
##     data = cps)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -11.409  -2.486  -0.631   1.872  35.021 
## 
## Coefficients:
##                     Estimate Std. Error t value Pr(>|t|)    
## (Intercept)           1.6508     6.7203   0.246  0.80605    
## education             0.8128     1.0869   0.748  0.45491    
## experience            0.2448     1.0818   0.226  0.82103    
## age                  -0.1580     1.0809  -0.146  0.88382    
## ethnicityhispanic    -0.6065     0.8696  -0.697  0.48584    
## ethnicityother       -0.8379     0.5745  -1.458  0.14532    
## regionsouth          -0.5627     0.4198  -1.340  0.18070    
## gendermale            1.9425     0.4194   4.631 4.60e-06 ***
## occupationoffice     -3.2682     0.7626  -4.286 2.17e-05 ***
## occupationsales      -4.0638     0.9159  -4.437 1.12e-05 ***
## occupationservices   -3.9754     0.8108  -4.903 1.26e-06 ***
## occupationtechnical  -1.3336     0.7289  -1.829  0.06791 .  
## occupationworker     -3.2905     0.8005  -4.111 4.59e-05 ***
## sectormanufacturing   0.5635     0.9915   0.568  0.57008    
## sectorother          -0.4774     0.9661  -0.494  0.62141    
## unionyes              1.6017     0.5127   3.124  0.00188 ** 
## marriedyes            0.3005     0.4112   0.731  0.46523    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 4.282 on 517 degrees of freedom
## Multiple R-squared:  0.3265, Adjusted R-squared:  0.3056 
## F-statistic: 15.66 on 16 and 517 DF,  p-value: < 2.2e-16
```

Look at the results, and interpret the estimates. Which variables are statistically significant?

Answer: `gender`, `occupation`, and `union` are the only variables that appear to be statistically significant.

Note: We can use the code `lm(wage ~ ., data = cps)` to estimate the above model, instead of typing every variable out. The `.` tells R to include every variable in the data set on the right-hand-side of the population model.

# Hypothesis tests {#hyp}

Using the above estimated model, test the hypothesis that the returns to education are zero. The null and alternative hypotheses are:

$H_0: \beta_{educ} = 0$

$H_A: \beta_{educ} \neq 0$

The t-statistic associated with this test is:

$t = \frac{b_{educ} - 0}{s.e.(b_{educ})}$

Take another look at the summary for `model2`, and note that $b_{educ} = 0.8128$ and that $s.e.(b_{educ}) = 1.0869$. The t-statistic is then:

```r
test1 <- 0.8128 / 1.0869
test1
```

```
## [1] 0.7478149
```

Notice `test1` pop up in the top-right environment window. Find this value in the `summary(model2)` table.

We need a p-value for this t-statistic, but first we need to know the degrees of freedom for this t-distribution. The sample size is 534 (see in the top-right), and we have estimated 17 $\beta$s (count them), so that the degrees of freedom are $534 - 17 = 517$. Find the degrees of freedom in the `summary(model2)` table.

Now our p-value is found by:

```r
(1 - pt(test1, 517)) * 2
```

```
## [1] 0.4549118
```

The `pt()` command gives the probability to the left of t-statistic. Since our t-statistic is greater than zero, we want the area to the right, so we have to use `1 - pt()`, and since our alternative hypothesis is two sided we need to multiply by two: `* 2`. Be sure that you can find this p-value in the `summary(model2)` table.

Notice that all tests of "no effect" have been automatically performed by R, but that any test where the $\beta$ takes a non-zero value under the null hypothesis needs to be calculated manually.
