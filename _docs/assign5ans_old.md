---
title: "Assignment 5 Answer Key"
permalink: /3040/assign5ansold/
excerpt: "Two-stage least-squares (IV) - Answer Key"
toc: false
---

------------------------------------------------------------------------

### Question 1

Load the fish market data:

```r
fish <- read.csv("https://rtgodwin.com/data/fish.csv")
```

The population model that we are trying to estimate is:

$\log (totqty) = \beta_0 + \beta_1 \log (avgprc) + \beta_2mon + \beta_3tues + \beta_4wed + \beta_5thurs + \epsilon$

The IV model that we estimated in the book (using the `ivreg()` function) was:

```r
install.packages("ivreg")
library(ivreg)
iv.fish <- ivreg(log(totqty) ~ log(avgprc) + mon + tues + wed + thurs |
                             wave2 + wave3 + mon + tues + wed + thurs,
                 data = fish)
summary(iv.fish)
```

```
Coefficients:
            Estimate Std. Error t value Pr(>|t|)    
(Intercept)  8.16410    0.18171  44.930  < 2e-16 ***
log(avgprc) -0.81582    0.32744  -2.492  0.01453 *  
mon         -0.30744    0.22921  -1.341  0.18317    
tues        -0.68473    0.22599  -3.030  0.00318 ** 
wed         -0.52061    0.22357  -2.329  0.02209 *  
thurs        0.09476    0.22521   0.421  0.67492    
```

We need to reproduce these results using the _two-stage least-squares_ (2SLS) approach. In the first stage, we regress the problematic _endogenous_ variable on the instruments and all regressors (using least-squares), and then save the LS _predicted values_:

```r
stage1.mod <- lm(log(avgprc) ~ wave2 + wave3 + mon + tues + wed + thurs,
                 data=fish)
logprice.fitted <- stage1.mod$fitted.values
```

In the second stage, we estimate the population model using least-squares, but we replace the variable $\log (avgprc)$ with the fitted values from the first stage:

```r
stage2.mod <- lm(log(totqty) ~ logprice.fitted + mon + tues + wed + thurs,
                 data=fish)
summary(stage2.mod)
```

```
Coefficients:
                Estimate Std. Error t value Pr(>|t|)    
(Intercept)      8.16410    0.18146  44.990  < 2e-16 ***
logprice.fitted -0.81582    0.32700  -2.495  0.01440 *  
mon             -0.30744    0.22890  -1.343  0.18259    
tues            -0.68473    0.22569  -3.034  0.00315 ** 
wed             -0.52061    0.22327  -2.332  0.02192 *  
thurs            0.09476    0.22490   0.421  0.67451    
---
```

The results are nearly identical to those obtained by the `ivreg()` function.
