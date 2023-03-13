---
title: "Lab 3"
permalink: /3040/lab3/
excerpt: "Multiple regression with CPS"
toc: false
---

------------------------------------------------------------------------

# Download a CPS data set {#cps}

Download the data from the website using:

```r
cps <- read.csv("https://rtgodwin.com/data/cps1985.csv")
```

This is a sub-sample from the 1985 wave of the "Current Population Survey" in the US, which contains information on individual wages and demographic characteristics. Take a good look at the data set either by clicking on the spreadsheet icon next to its object name in the top-right window, or by using the command `View(cps)`. I'll just take a look at the first 6 observations for each variable:

```r
head(cps)
```

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

# Review: _single_ hypothesis test (t-test) {#hyp}

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

# Multiple hypothesis test (F-test)

Education, experience, and age are variables that are highly multicollinear (see Section 6.4.2 in the textbook on imperfect multicollinearity). They _appear_ to be statistically insignificant (according to their large p-values). If we want to drop them from the model, we need to test to see if they are _jointly_ insignificant:

$H_0: \beta_{education} = 0 \text{ and } \beta_{experience} = 0 \text{ and } \beta_{age} = 0$

$H_A: \text{not } H_0$

- Since it is a _joint_ hypothesis (the hypothesis includes multiple restrictions on the $\beta$), we need to use an F-test.
- To calculate the F-statistic, we can estimate two different models, and compare them using the `anova()` function.
- One model we need to estimate is the _unrestricted_ model (the model under the alternative hypothesis).
- The other model we need to estimate is the restricted model (under the null hypothesis).

## Unrestricted model

This has already been estimated as `model2`.

## Restricted model

We take the unrestricted model, and substitute in the values for the $\beta$ that have been specified in $H_0$:

```r
model2.restricted <- lm(wage ~ ethnicity + region + gender + occupation + sector
                        + union + married, data = cps)
```

## `anova()`

We put both estimated models into the `anova()` function:

```r
anova(model2, model2.restricted)
```

```
Analysis of Variance Table

Model 1: wage ~ education + experience + age + ethnicity + region + gender + 
    occupation + sector + union + married
Model 2: wage ~ ethnicity + region + gender + occupation + sector + union + 
    married
  Res.Df     RSS Df Sum of Sq      F    Pr(>F)    
1    517  9480.8                                  
2    520 10396.0 -3   -915.19 16.636 2.481e-10 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
```

The F-statistic is 16.636. According to the table below, the 5% critical value is 2.60 (we have three restrictions in $H_0$, so $q = 3$). Since the F-stat is greater than 2.60, we reject the null that _education_, _experience_, and _age_, have no effect on wage.

We can also look at the p-value to make our "reject" or "fail to reject" decision. The p-value for this test is `2.481e-10` or 0.0000000002481. We reject the null at any common significance level.

The results of this test indicate that we should not drop these three variables from the model

<div align="center">

<font size = "4">

<table>
<thead>
<tr class="header">
<th style="text-align: center;">q</th>
<th style="text-align: center;">5% critical value</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: center;">1</td>
<td style="text-align: center;">3.84</td>
</tr>

<tr class="odd">
<td style="text-align: center;">2</td>
<td style="text-align: center;">3.00</td>
</tr>
  
<tr class="odd">
<td style="text-align: center;">3</td>
<td style="text-align: center;">2.60</td>
</tr>
  
<tr class="odd">
<td style="text-align: center;">4</td>
<td style="text-align: center;">2.37</td>
</tr>

<tr class="odd">
<td style="text-align: center;">5</td>
<td style="text-align: center;">2.21</td>
</tr>  
  
</tbody>
</table>  

</font>
</div>

# Another F-test

Test the joint significance of the `ethnicity`, `sector`, and `married` variables (which all appear to be statistically insignificant according to the t-tests):

```r
model3 <- lm(wage ~ education + experience + age + region + gender 
             + occupation + union, data = cps)
anova(model2, model3)
```

```
Analysis of Variance Table

Model 1: wage ~ education + experience + age + ethnicity + region + gender + 
    occupation + sector + union + married
Model 2: wage ~ education + experience + age + region + gender + occupation + 
    union
  Res.Df    RSS Df Sum of Sq      F Pr(>F)
1    517 9480.8                           
2    522 9599.2 -5   -118.44 1.2918 0.2659
```

With a p-value of 0.2659, we fail to reject the null that these variables have no effect on wage. This suggests that we might drop the variables from the model. The final estimated model is:

```r
summary(model3)
```

```
Coefficients:
                    Estimate Std. Error t value Pr(>|t|)    
(Intercept)           0.7047     6.6569   0.106 0.915739    
education             0.7682     1.0865   0.707 0.479852    
experience            0.1896     1.0818   0.175 0.860961    
age                  -0.0958     1.0808  -0.089 0.929402    
regionsouth          -0.6878     0.4155  -1.655 0.098503 .  
gendermale            1.8471     0.4161   4.439 1.11e-05 ***
occupationoffice     -3.3463     0.7608  -4.398 1.32e-05 ***
occupationsales      -3.9753     0.9151  -4.344 1.68e-05 ***
occupationservices   -4.1478     0.8061  -5.145 3.78e-07 ***
occupationtechnical  -1.2713     0.7287  -1.745 0.081651 .  
occupationworker     -2.7993     0.7573  -3.697 0.000242 ***
unionyes              1.5180     0.5089   2.983 0.002988 ** 
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 4.288 on 522 degrees of freedom
Multiple R-squared:  0.3181,	Adjusted R-squared:  0.3037 
F-statistic: 22.13 on 11 and 522 DF,  p-value: < 2.2e-16
```
