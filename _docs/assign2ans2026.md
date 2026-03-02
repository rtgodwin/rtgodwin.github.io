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

This question is asking students to estimate the $$\beta$$ in a model such as:

$$Sales = \beta_0 + \beta_1Score + \epsilon$$

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
