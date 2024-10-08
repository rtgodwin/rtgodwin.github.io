---
title: "7010 Assignment 2"
permalink: /7010/assign2/
excerpt: "Assignment 2"
toc: false
sidebar:
  nav: "pages"
---

**Due: Nov. 5th**. Worth 3% of your mark. Upload your answers
to the assignment 2 dropbox on UM Learn.

------------------------------------------------------------------------

1.  Exercise question 7, from Chapter 5.

    OR

    Run the following code several times, changing just _one thing_ each time you run it. Do this in order to show that the LS estimator is _consistent_. Your answer should include several histograms.

    ```r
    nrep <- 1000 # The number of times the experiment will run.
    n <- 100 # Set the sample size.
    beta1 <- 3 # Set the true value for the intercept.
    beta2 <- -2 # Set the true value for the slope.
    x <- rnorm(n) # Generates a Normally distributed regressor.
    b2 <- numeric(nrep) # Setup an empty vector to store the LS estimates.

    for(i in 1:nrep) { # Start the simulation loop.
      y <- beta1 + beta2 * x + rnorm(n) # Generate the y variable according to the true population model.
      b2[i] <- lm(y ~ x)$coefficients[2] # Estimate and store the value for b2
    }

    hist(b2, xlim = c(-2.5,-1.5)) # View the simulated sampling distribution
    ```
    
3.  Any exercise question that does not include an answer key.
