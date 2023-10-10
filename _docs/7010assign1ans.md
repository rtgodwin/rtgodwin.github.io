---
title: "7010 Assignment 1 Answers"
permalink: /7010/assign1ans/
excerpt: "Assignment 1 Answer Key"
toc: false
sidebar:
  nav: "pages"
---

1.  Download any data set that contains at least 3 variables.

I use a fake dataset that I created:

```r
mydata <- read.csv("https://rtgodwin.com/data/monopolist.csv")
```

2.  In R, use LS to estimate a population model of the form:
    $$\boldsymbol{y} = X\boldsymbol{\beta} + \boldsymbol{\epsilon}
    \nonumber$$

    On the right hand side of your model, only include 2 regressors and
    the intercept (like in the Cobb-Douglas example).

3.  In R, use LS to estimate a population model of the form:
    $$\boldsymbol{y} = X\boldsymbol{\beta} + \boldsymbol{\epsilon}
    \nonumber$$

    On the right hand side of your model, only include 2 regressors and
    the intercept (like in the Cobb-Douglas example).

4.  Verify that the $x$ variables are orthogonal to the LS residuals.

5.  Verify that the LS residuals from your estimated model sum to zero.

6.  Verify that the regression line (it is actually a 2-dimensional
    "plane") passes through the sample mean of the data.

7.  Verify that the fitted values and residuals are invariant to a
    non-singular linear transformation.

8.  Use the Frisch-Waugh-Lovell theorem and partial regression to get
    the LS estimate for just one of the $\beta$.

    Recall that the FWL theorem suggests that, for the model:

    $$y = \beta_0 + \beta_1x_1 + \beta_2x_2 + \epsilon
    \nonumber$$

    the LS estimator for $\beta_2$ (for example) can be obtained by:

    1.  Regressing $x_2$ on $x_1$ and the constant, saving the
        residuals.

    2.  Regressing $y$ on $x_1$ and the constant, saving the residuals.

    3.  Regressing the residuals from (ii) onto (i), without a constant.

------------------------------------------------------------------------

For the R code required for this assignment, click on [this
tutorial](http://home.cc.umanitoba.ca/~godwinrt/7010/assigntutorial1.html).
