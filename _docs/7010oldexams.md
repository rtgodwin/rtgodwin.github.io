---
title: "ECON 7010 - Previous Exams"
permalink: /7010/oldexams
excerpt: "ECON 7010 exams"
last_modified_at:
toc: false
sidebar:
  nav: "pages"
---

## Previous midterms

For the 2025 Fall midterm: the exam covers everything up to "Critical Values" (slide 19 in simple hypothesis testing)

Ignore the following questions when you review past midterms:
- 2022 Fall: 5, 6
- 2019 Midterm 2: All questions except 2
- 2016 midterm 2
- 2015 midterm 2
- 2014 midterm 2

[Midterm Formula Sheet](https://rtgodwin.com/7010/exams/midformula.pdf)

| Year                                                  | Answer Key
| :---------------------------------------------------: | :--------------------------------------------------------: |
| [2023](https://rtgodwin.com/7010/exams/mid2023ans.pdf)   |   |
| [2022](https://rtgodwin.com/7010/exams/mid2022fallans.pdf)   |   |
| [2022](https://rtgodwin.com/7010/exams/2022midans.pdf)   |   |
| [2020](https://rtgodwin.com/7010/exams/2020mid.pdf)   |   |
| [2019 Mid 2](https://rtgodwin.com/7010/exams/2019mid2.pdf)   |                                                            |
| [2019 Mid 1](https://rtgodwin.com/7010/exams/2019mid1.pdf)   | [Answers](https://rtgodwin.com/7010/exams/2019mid1ans.pdf)  |
| [2016 Mid 2](https://rtgodwin.com/7010/exams/2016mid2.pdf) | [Answers](https://rtgodwin.com/7010/exams/2016mid2ans.pdf) |
| | [Final question](https://rtgodwin.com/7010/exams/2016mid2ans2.pdf) |
| [2016 Mid 1](https://rtgodwin.com/7010/exams/2016mid1.pdf) | [Answers](https://rtgodwin.com/7010/exams/2016mid1ans.pdf) |
| [2015 Mid 2](https://rtgodwin.com/7010/exams/2015mid2.pdf)   | [Answers](https://rtgodwin.com/7010/exams/2015mid2ans.pdf)  |
| [2015 Mid 1](https://rtgodwin.com/7010/exams/2015mid1.pdf)   | [Answers](https://rtgodwin.com/7010/exams/2015mid1ans.pdf)  |
| [2014 Mid 2](https://rtgodwin.com/7010/exams/2014mid2.pdf)   | [Answers](https://rtgodwin.com/7010/exams/2014mid2ans.pdf)  |
| [2014 Mid 1](https://rtgodwin.com/7010/exams/2014mid1.pdf)   | [Answers](https://rtgodwin.com/7010/exams/2014mid1ans.pdf)  |
| [2013](https://rtgodwin.com/7010/exams/2013mid.pdf)   | [Answers](https://rtgodwin.com/7010/exams/2013midans.pdf)  |

## Previous finals

[Final Formula Sheet](https://rtgodwin.com/7010/exams/formula.pdf)

| Year                                                  | Answer Key
| :---------------------------------------------------: | :---------------------------------------------------------: |
| [2023](https://rtgodwin.com/3040/exams/2023finalans.pdf) |  |
| [2022](https://rtgodwin.com/7010/exams/2022final.pdf) | [Answers](https://rtgodwin.com/7010/exams/2022finalans.pdf) |
| [2022 Winter](https://rtgodwin.com/7010/exams/2022finalW.pdf) |  |
| [2020](https://rtgodwin.com/7010/exams/2020final.pdf) | [Answers](https://rtgodwin.com/7010/exams/2020finalans.pdf) |
| [2019](https://rtgodwin.com/7010/exams/2019final.pdf) | [Answers](https://rtgodwin.com/7010/exams/2019finans.pdf) |
| [2016](https://rtgodwin.com/7010/exams/2016final.pdf) | [Answers](https://rtgodwin.com/7010/exams/2016finalans.pdf) |
| [2015](https://rtgodwin.com/7010/exams/2015final.pdf) | [Answers](https://rtgodwin.com/7010/exams/2015finalans.pdf) |
| [2014](https://rtgodwin.com/7010/exams/2014final.pdf) | [Answers](https://rtgodwin.com/7010/exams/2014finalans.pdf) |
| [2013](https://rtgodwin.com/7010/exams/2013final.pdf) | [Answers](https://rtgodwin.com/7010/exams/2013finalans.pdf) |

## Proofs that we skipped. References are to the lecture slides.
 - Distribution of the t-test statistic: slides 8, pg. 18
 - Khinchin's theorem, weak law of large numbers for proving the consistency of $s^2$: slides 9, pg. 14
 - Deriving the RLS estimator: slides 11, pg. 21-22
 - RLS estimator has smaller variance than LS estimator: slides 11, pg. 26-28
 - Joining the spline at the knots: slides 12, pg. 10
 - Deriving the Newton-Raphson by Taylor series approximation: slides 12, pg. 16-17

# Course overview (post midterm)

## Chapter 7: Asymptotics
 - Why consider asymptotic properties of estimators?
 - What is consistency? (two versions)
 - The plim() operator and the consistency of various estimators ($b$, $\hat{\sigma}^2$, $s^2$)
 - Asymptotic efficiency, and why the $\sqrt{n}$ is needed to avoid a "spike"

## Chapter 8: IV
 - The missing variable problem, and the resulting inconsistency of LS
 - Two properties to make an instrument "valid"
 - Some intuition behind how the instrument is used
 - The over- and just-identified IV estimator
 - 2SLS procedure
 - Testing (Hausman, exogeneity, weak instruments)
 - Returns to education example

## Chapter 9: Multiple hypotheses
 - What a multiple hypothesis is
 - Why t-tests can't be used
 - $R$ and $q$ matrices
 - Wald test, then the F-test (and which one to use)
 - The RLS estimator and what it tells us about "imposing restrictions" on a model
 - Conducting an F-test using $R^2$ from a restricted and an unrestricted model
 - Testing for differences

## Chapter 10: Non-linear effects and NLS
 - Avoiding NLS and keeping LS: linearizing and polynomial approximation
 - Reasons to estimate non-linear model directly (for example gravity model)
 - Definition of NLS estimator
 - Why a numerical algorithm is typically needed
 - How the Newton algorithm works
 - Convergence, iterations, tolerance
 - Issues in estimation (singularity, cycling, etc.)

## Chapter 11: Heteroskedasticity
 - Define het.
 - Consequences of het. (inconsistent s.e., LS is inefficient)
 - Testing for het.
 - Fixing het.
 - The GLS estimator
 - Clustering (group sizes determine $P$ matrix, apply GLS directly)

## Chapter 12: Time series
 - AR process
 - variance of an AR(1) error term
 - $\lvert \rho \rvert < 1$ and $\lvert \rho \rvert = 1$
 - random walks and spurious regressions
