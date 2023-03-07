---
title: "Do Republicans cause gun deaths?"
permalink: /3040/guns/
excerpt: 
toc: false
---

------------------------------------------------------------------------

Indirectly "yes", but I will ultimately argue "no".

> People don't kill people, guns kill people.

## Introduction

Lots of people in the US are killed by guns. Guns are a leading cause of death among children. Mass shootings happen nearly daily. Gun laws and controls are a hotly debated and divisive political issue in the US. (Each of these statements should have a citation if this was serious research). In other parts of the world, it is obvious to us that they are crazy with their guns and that they need to stop.

I will use data on gun deaths to illustrate _Omitted Variable Bias_, and to also explore the link between gun ownership and gun deaths.

## Description of the data

The data is for US states in 2016, and comes from various sources.

<div align="center">

<font size = "4">

<table>
<thead>
<tr class="header">
<th style="text-align: center;">Variable</th>
<th style="text-align: center;">Description</th>
<th style="text-align: center;">Source</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: left;">death.per.100k</td>
<td style="text-align: left;">The number of gun deaths per 100,000 people, in 2016.</td>
<td style="text-align: left;"><a href="https://www.cdc.gov/nchs/pressroom/sosmap/firearm_mortality/firearm.htm">CDC</a></td>
</tr>
  
<tr class="even">
<td style="text-align: left;">gun.owner</td>
<td style="text-align: left;">The estimated percentage of households in 2016 that owned guns.</td>
<td style="text-align: left;"><a href="https://www.rand.org/pubs/tools/TL354.html">RAND</a></td>
</tr>
  
<tr class="odd">
<td style="text-align: left;">trump.vote</td>
<td style="text-align: left;">The percentage of votes for Trump in 2016. This variable is meant to measure the percentage of Repbulicans by state.</td>
<td style="text-align: left;"><a href="https://www.nytimes.com/elections/2016/results/president">New York Times</a></td>
</tr> 

<tr class="even">
<td style="text-align: left;">back.check</td>
<td style="text-align: left;">A dummy variable indicating whether the state has background checks.</td>
<td style="text-align: left;"><a href="https://www.rand.org/pubs/tools/TL354.html">RAND</a></td>
</tr>
</tbody>
</table>  

</font>
</div>

## Download the data

Download the data from the website using:

```r
guns <- read.csv("https://rtgodwin.com/data/guns.csv")
```

Take a look at the first 6 observations for each variable:

```r
head(guns)
```

```
       state death.per.100k gun.owner trump.vote back.check permit
1    Alabama           21.5      52.8       62.1          0      0
2     Alaska           23.3      57.2       51.3          0      0
3    Arizona           15.2      36.0       48.1          0      0
4   Arkansas           17.8      51.8       60.6          0      0
5 California            7.9      16.3       31.5          1      1
6   Colorado           14.3      37.9       43.3          1      0
```

- Notice that Alabama has a lot of deaths (21.5) and Republicans (62.1\%).
- California has few deaths (7.9) and Republicans (31.5\%). 
- Think about the magnitude of these numbers. With a population of 39.15 million in 2016, 7.9 deaths per 100k means 3,093 deaths.

## A link between Republicans and gun deaths?

Plot the data:

```r
plot(guns$trump.vote, guns$death.per.100k,
     xlab = "Trump vote percentage in 2016",
     ylab = "Gun deaths per 100k population",
     main = "Deaths and votes by state",
     pch = 16, col = "magenta")
```

![](https://rtgodwin.com/3040/images/guns.png)

Now we'll estimate the model:

$death.per.100k = \beta_0 + \beta_1trump.votes + \epsilon$

In R we can use:

```r
mod1 <- lm(death.per.100k ~ trump.vote, data = guns)
summary(mod1)
```

```
Coefficients:
            Estimate Std. Error t value Pr(>|t|)    
(Intercept) -2.95296    2.57444  -1.147    0.257    
trump.vote   0.32572    0.05131   6.348 7.36e-08 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 3.67 on 48 degrees of freedom
Multiple R-squared:  0.4564,	Adjusted R-squared:  0.4451 
F-statistic:  40.3 on 1 and 48 DF,  p-value: 7.357e-08
```

- The estimated effect of 0.32572 means that an additional 1% vote for Trump is associated with an extra 0.33 gun deaths per 100,000 population.
- To try to put this in perspective, in California this would be an extra 129 gun deaths in 2016.
- The p-value on `trump.vote` is small (0.0000000736). We reject the null that percentage of Republicans have no association with gun deaths. According to this model, `trump.vote` is statistically significant.

The above estimated effect of `trump.vote` on gun deaths is likely biased. This is a situation of Omitted Variable Bias (OVB). What do you think the most important determinant of gun deaths is?

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
