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
