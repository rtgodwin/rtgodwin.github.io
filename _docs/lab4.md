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

$$DALE = \beta_0 + \beta_1 \log(HEXP) + \beta_2 HC3 + \beta_3HC3^2 + \beta_4HC3^3 + \beta_5OECD + \beta_6GINI + \beta_7TROPICS + \beta_8POPDEN + \beta_9PUBTHE$$

$$+ \beta_{10}GDPC + \beta_{11}VOICE + \beta_{12}GEFF + \beta_{13}OECD \times log(HEXP) + \beta_{14}OECD \times HC3 + \beta_{15}OECD \times HC3^2 + \beta_{16}OECD \times HC3^3 $$


