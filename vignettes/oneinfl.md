---
layout: custom-layout
title: "Package oneinfl"
permalink: /oneinfl/
excerpt: 
toc: false
---

------------------------------------------------------------------------

# Package `oneinfl` 

The R package `oneinfl` estimates one-inflated positive Poisson (OIPP) and one-inflated zero-truncated (OIZTNB) regression models. When count data are truncated so that $y = 1,2,\dots$, it is also often inflated at $y=1$. The current standard model for treating such data is the zero-truncated negative binomial (ZTNB) model. ZTNB fails to account for excess 1s (or too few 1s), resulting in biased and inconsistent estimators.

This vignette illustrates `oneinfl` by reproducing and extending the MedPar results in "One-inflated zero-truncated count regression models" (Godwin, 2023). Please cite this paper when using `oneinfl`.

Functions in this package:
- `oneinfl(formula, data, dist)` - estimate the new OIZTNB and OIPP models
- `truncreg(formula, data, dist)` - estimate the standard ZTNB and PP models
- `oneLRT(model1, model2)` - likelihood ratio test for overdispersion or one-inflation
- `summary.oneinfl(model)` - create a summary table of estimated parameters, standard errors, z-statistics and _p_-values, estimated average and average absolute one-inflation, and log-likelihood

## Load package and data

Load the `oneinfl` package using:

```r
devtools::install_github("rtgodwin/oneinfl")
library(oneinfl)
```

Load the medpar data from the `msme` package:
```r
library(msme)
data(medpar)
data = medpar
```

## Estimate OIZTNB and OIPP using `oneinfl(formula, data, dist)`

Estimate the one-inflated zero-truncated negative binomial (OIZTNB) model:

```r
formula <- los ~ white + died + type2 + type3 | white + died + type2 + type3
OIZTNB <- oneinfl(formula, data, dist="negbin")
OIPP <- oneinfl(formula, data, dist="Poisson")
```

`formula` is the population model to be estimated, variables that precede `|` link to the mean function and variables that follow `|` link to one-inflation. `data` is a data frame and `dist="negbin` estimated OIZTNB while `dist=Poisson` estimates OIPP.

## Estimate zero-truncated negative binomial (ZTNB) and positive Poisson (PP) models using `truncreg(formula, data, dist)`

These are the current standard models for treating zero-truncated count data. Estimate them in `oneinfl` using:

```r
formula <- los ~ white + died + type2 + type3
ZTNB <- truncreg(formula, data, dist="negbin")
PP <- truncreg(formula, data, dist="Poisson")
```
## Test for overdispersion and one-inflation using `oneLRT(model1, model2)`

`oneLRT` extracts the log-likelihood and number of parameters in any two models estimated by `oneinfl` or `truncreg`. It returns the likelihood ratio test statistic and its associated _p_-value. It can be used to test hypotheses involving nested models.

### Overdispersion

Likelihood ratio test for overdispersion:

```r
oneLRT(OIZTNB, OIPP)
```
```
$LRTstat
[1] 3305.341

$pval
[1] 0
```
We reject the null hypothesis of no overdispersion. Overdispersion is a well-developed topic that has led many to discard a Poisson model in favour of a negative binomial model. The principle extends to both truncated data (the ZTNB model vs. the PP model) and one-inflation (OIZTNB vs. OIPP).

### One-inflation

Likelihood ratio test for one-inflation:

```r
oneLRT(OIZTNB, ZTNB)
```

```
$LRTstat
[1] 131.4165

$pval
[1] 0
```

The LRT supports the presence of one-inflation (the null hypothesis is no one-inflation).

## Test for one-inflation using `oneWald(oneinfl.model)`

Godwin (2023) presents a Wald test for the presence of one-inflation. Only the one-inflated models need be estimated; `oneinfl.model` should be a OIZTNB or OIPP model estimated by `oneinfl`.

```r
oneWald(OIZTNB)
```

```
$W
[1] 469.1062

$pval
[1] 0
```

The Wald test also supports the presence of one-inflation.

## Summarize using `summary.oneinfl(model)`

`summary.oneinfl(model)` is a custom summary function for OIZTNB, OIPP, ZTNB, and PP models estimated using `oneinfl`, which provides output similar to standard uses of `summary()`. For the OIZTNB model:

```r
summary.oneinfl(OIZTNB)
```

```
Call:
formula:  los ~ white + died + type2 + type3 | white + died + type2 + type3 
distribution:  negbin 

Coefficients (beta):
             Estimate Std.Error z_value   p.value    
b(Intercept)  2.29913   0.07184  32.005 0.000e+00 ***
bwhite       -0.09708   0.07138  -1.360 1.738e-01    
bdied        -0.06814   0.04492  -1.517 1.293e-01    
btype2        0.23413   0.05349   4.377 1.204e-05 ***
btype3        0.75580   0.07884   9.586 0.000e+00 ***

Coefficients (gamma):
             Estimate Std.Error z_value p.value    
g(Intercept)  -4.2002    0.5060  -8.300 0.00000 ***
gwhite         0.6594    0.4814   1.370 0.17075    
gdied          2.3349    0.2359   9.899 0.00000 ***
gtype2        -0.5413    0.2829  -1.913 0.05569   .
gtype3        -0.7507    0.4472  -1.679 0.09324   .

alpha:
  Estimate Std.Error z_value p.value    
1    2.267     0.146   15.53       0 ***

Signif. codes:  0 `***' 0.001 `**' 0.01 `*' 0.05 `.' 0.1 ` ' 1

average one-inflation:  0.0416855179433309 

average absolute one-inflation:  0.0680909723883016 

Log-likelihood:  -4671.06423844655
```

For example, when the variable $died = 1$, one-inflation increases significantly, but $died$ is not significant 

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
<td style="text-align: left;">trump.vote</td>
<td style="text-align: left;">The percentage of votes for Trump in 2016. This variable is meant to measure the percentage of Repbulicans by state.</td>
<td style="text-align: left;"><a href="https://www.nytimes.com/elections/2016/results/president">New York Times</a></td>
</tr>   
  
<tr class="odd">
<td style="text-align: left;">gun.owner</td>
<td style="text-align: left;">The estimated percentage of households in 2016 that owned guns.</td>
<td style="text-align: left;"><a href="https://www.rand.org/pubs/tools/TL354.html">RAND</a></td>
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

- Notice that Alabama has a lot of deaths (21.5) and Republicans (62.1%).
- California has few deaths (7.9) and Republicans (31.5%). 
- Think about the magnitude of these numbers. With a population of 39.15 million in 2016, 7.9 deaths per 100k means 3,093 deaths.
- Look at how many households have guns!

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
- The $R^2$ is 46%.

The above estimated effect of `trump.vote` on gun deaths is likely biased. This is a situation of Omitted Variable Bias (OVB). What do you think the most important determinant of gun deaths is?
{: .notice--danger}

## A model that uses the percentage of households that have guns

We'll estimate a model that includes more variables (`gun.owner` and `back.check`). The new population model is:

$death.per.100k = \beta_0 + \beta_1trump.votes + \beta_2gun.owner + \beta_3back.check + \epsilon$

In R we can use:

```r
mod2 <- lm(death.per.100k ~ trump.vote + gun.owner + back.check, data = guns)
summary(mod2)
```

```
Coefficients:
            Estimate Std. Error t value Pr(>|t|)    
(Intercept)  1.74293    2.87344   0.607 0.547121    
trump.vote   0.08054    0.07435   1.083 0.284361    
gun.owner    0.19353    0.05075   3.814 0.000406 ***
back.check  -1.26643    1.23944  -1.022 0.312231    
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 3.198 on 46 degrees of freedom
Multiple R-squared:  0.6044,	Adjusted R-squared:  0.5786 
F-statistic: 23.42 on 3 and 46 DF,  p-value: 2.37e-09
```

`trump.vote` is no longer statistically significant!
{: .notice--danger}

- We fail to reject the null that percentage of Replubicans have no effect on gun deaths.
- However, the percentage of gun owners has a statistically significant effect: we reject the null that `gun.owner` has no effect on deaths.
- It is estimated that for every additional 1% of households that own guns, an extra 0.19 people died per 100k.
- Backgorund checks have a negative, but statistically insignificant impact.

## Summary

After including `gun.ownership`, `trump.vote` is no longer statistically significant. On einterpretation of the above estimated results is that the percentage of Republicans only effects gun deaths _through_ the tendency of Republicans to have more guns. That is, after controlling for the amount of guns, it doesn't matter whether a people are Republican or not. That is, two state with differing percentages of Republicans, but with the same percentage of guns, would have the same amount of gun deaths. 

