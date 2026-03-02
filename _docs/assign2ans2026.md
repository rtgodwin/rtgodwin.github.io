---
title: "Assignment 1 Answer Key"
permalink: /3040/assign1ans/
excerpt: "Sales from different video game consoles - Answer Key"
toc: false
---

------------------------------------------------------------------------

### Question 1

There are several ways to create the subsample, but the one recommended in class is:

```r
sub <- subset(mydata, Platform == "PS4" | Platform == "XOne" | Platform == "Wii")
```

### Question 2

Students should use code that looks something like:

```r
plot(sub$Score, sub$Sales)
```

and include the scatterplot in their assignment.

### Question 3

This question is asking students to estimate the $$\beta_1$$ in a model such as:

$$Sales = \beta_0 + \beta_1Score + \epsilon$$

They could estimate the model using something like:

```r
mod1 <- lm(Sales ~ Score, data = sub) 
summary(mod1)
```
Then, using the output:
```
Call:
lm(formula = Sales ~ Score, data = sub)

Residuals:
   Min     1Q Median     3Q    Max 
-3.280 -1.833 -1.038  0.222 80.462 

Coefficients:
            Estimate Std. Error t value Pr(>|t|)    
(Intercept)  -2.8895     1.1321  -2.552    0.011 *  
Score         0.6867     0.1560   4.401 1.32e-05 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 5.155 on 497 degrees of freedom
Multiple R-squared:  0.03751,	Adjusted R-squared:  0.03557 
F-statistic: 19.37 on 1 and 497 DF,  p-value: 1.319e-05
```

should say something like: "Video games that score 1 point higher, make an additional $686,700 in sales, on average."

### Question 4

The R-square of 0.03751 (or 0.03557) should be interpreted as: "Score explains only 3.8% of the variation in video game sales."

### Question 5

This hypothesis has already been tested in the `summary()` function above from question 3. The t-stat is 4.401, and p-value is 0.0000132. The null hypothesis is rejected.

### Question 6

There are several ways students might approach this. Here is a possible way:

```r
mod2 <- lm(Sales ~ Score + Platform, data = sub)
summary(mod2)
```

```
Call:
lm(formula = Sales ~ Score + Platform, data = sub)

Residuals:
   Min     1Q Median     3Q    Max 
-3.717 -1.816 -0.922  0.238 80.431 

Coefficients:
             Estimate Std. Error t value Pr(>|t|)    
(Intercept)   -2.5415     1.3944  -1.823    0.069 .  
Score          0.6965     0.1651   4.218 2.93e-05 ***
PlatformWii   -0.3925     0.6188  -0.634    0.526    
PlatformXOne  -1.2743     0.8618  -1.479    0.140    
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 5.154 on 495 degrees of freedom
Multiple R-squared:  0.04179,	Adjusted R-squared:  0.03598 
F-statistic: 7.196 on 3 and 495 DF,  p-value: 9.792e-05
```

Students may exclude the `Score` variable, that's ok. The important part is that students are able to interpret the coefficients on the dummy variables. For example, because `PS4` is the base group, `Wii` games are selling $392,500 less on average compared to `PS4`, and 

Answers should be presented in a table:

Table 1: _Sales_ and _critic score_ summary statistics for Xbox One ($n=57$) and Playstation 4 ($n=96$) video games.

|       |               | sample mean | sample var. |  min |  max  |
|:-----:|:-------------:|:-----------:|:-----------:|:----:|:-----:|
| Sales |      Xbox One |     1.66    |     2.77    | 0.02 |  8.72 |
|       | Playstation 4 |     2.91    |    12.77    | 0.01 | 19.39 |
| Score |      Xbox One |     7.85    |     0.83    |  5.1 |  9.4  |
|       | Playstation 4 |     7.82    |     1.80    |  1.0 |  10.0 |

  
There are many ways to get the answers for the above table. One such way is:

```r
sum(sub$Platform == "XOne")
sum(sub$Platform == "PS4")

summary(sub$Sales[sub$Platform == "XOne"])
var(sub$Sales[sub$Platform == "XOne"])
summary(sub$Sales[sub$Platform == "PS4"])
var(sub$Sales[sub$Platform == "PS4"])

summary(sub$Score[sub$Platform == "XOne"])
var(sub$Score[sub$Platform == "XOne"])
summary(sub$Score[sub$Platform == "PS4"])
var(sub$Score[sub$Platform == "PS4"])
```

### Question 4

The correlation is 0.39. This is obtained from:

```r
cor(sub$Sales, sub$Score)
```

You might also calculate the correlation separately by XBox One and PS4 games, and that is fine. In this case you would use:

```r
cor(subX$Sales, subX$Score)
cor(subP$Sales, subP$Score)
```

### Question 5

You need to include an actual scatterplot in your answer. The scatterplot can be obtained using:

```r
#Create the variable to control the colour
sub$pointcol <- "red"
sub$pointcol[sub$Platform == "XOne"] <- "blue"

#Now create the plot:
plot(x = sub$Score, y = sub$Sales, col = sub$pointcol, pch = 16,
main = "Sales␣and␣critic␣scores␣for␣PS4␣and␣Xbox␣One␣games",
xlab = "Scores",
ylab = "Sales")

# Add the legend
legend("topleft",
legend = c("PS4", "Xbox␣One"),
col=c("red", "blue"), pch=16)
```

### Question 6

There are many ways to answer this question. I am looking for any attempt to quantify the relationship between games that receive a higher critic score, and the revenue made for the game.
