---
title: "7010 Assignment 1 Answers"
permalink: /7010/assign1ans/
excerpt: "Assignment 1 Answer Key"
toc: false
sidebar:
  nav: "pages"
---

1.  Download any data set that contains at least 3 variables.

    ---
    I use a fake dataset that I created:
    ```r
    mydata <- read.csv("https://rtgodwin.com/data/monopolist.csv")
    ```
    ---
2.  In R, use LS to estimate a population model of the form:
    $$\boldsymbol{y} = X\boldsymbol{\beta} + \boldsymbol{\epsilon}
    \nonumber$$

    On the right hand side of your model, only include 2 regressors and
    the intercept (like in the Cobb-Douglas example).

    ---
    I estimate the model using:
    ```r
    mod <- lm(quantity ~ price + bad.weather, data = mydata)
    ```
    ---

3.  Verify that the $x$ variables are orthogonal to the LS residuals.

    ---
    I save and extract the residuals from the estimated model:
    ```r
    residuals <- mod$residuals
    ```
    The question asks to show that $X^{\prime} e = 0$. To transpose a vector in R, we can use `t()`, and `%*%` multiplies matrices (or vectors):
    ```r
    t(residuals) %*% mydata$price
    >>               [,1]
    >> [1,] -4.085621e-14
    ```
    `-4.085621e-14` is 0.000000000000004085621. This is not quite zero due to rounding. We can also verify that the other variable is orthogonal as well:
    ```r
    t(residuals) %*% mydata$bad.weather
    >>               [,1]
    >> [1,] -1.498801e-15
    ```
    You can also calculate $X^{\prime} e$ by taking the "inner product": `sum(residuals * mydata$price)`.
    
    Many students attempted to show orthogonality in different ways, which is fine, as long as the "others ways" are equivalent to orthogonality. For example, you can check the correlation between the $X$ variables and the residuals and show that it is "close" to zero, because when one of the variables has mean zero (in this case the residuals), then correlation and orthogonality are equivalent. So, using `cor(residuals, mydata$price)` works as well.

    ---
5.  Verify that the LS residuals from your estimated model sum to zero.

    ---
    Sum the residuals and see if they are "close" to zero ("close" due to rounding):
    ```r
    sum(residuals)
    >> [1] -1.991463e-15
    ```
    ---
        
7.  Verify that the regression line (it is actually a 2-dimensional
    "plane") passes through the sample mean of the data.

8.  Verify that the fitted values and residuals are invariant to a
    non-singular linear transformation.

9.  Use the Frisch-Waugh-Lovell theorem and partial regression to get
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
