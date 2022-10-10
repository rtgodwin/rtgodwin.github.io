---
title: "Lab 2"
permalink: /3040/lab2/
excerpt: "Monopolist's Demand Curve"
toc: true
---

------------------------------------------------------------------------

# Background Story: A Monopolist's Demand Curve

On Mars, Jim is the only person selling Earth rocks. There is no substitute for authentic Earth rocks, so Jim is a monopolist. Jim controls the price. Jim has tried randomly changing the price of Earth rocks 132 times; for each price change, Jim recorded the quantity (in cubic cm) of Earth rocks sold for that day. Load the data into RStudio:

```r
rocks <- read.csv("https://rtgodwin.com/data/monopolist.csv")
```

# Explore the data

Look at the data in a spreadsheet (close the spreadsheet when done):

```r
View(rocks)
```    

There are three variables in the data. `quantity` is the amount of moon rocks sold on a day, `price` is the price set by the monopolist for that day, and `bad.weather` is a dummy variable equal to 1 if the weather on Mars was bad for that day, or equal to 0 if the weather was good.

Create a new variable to control the colour of each data point, based on the `bad.weather` dummy variable:

```r
rocks$mycol[rocks$bad.weather == 1] <- "orange"
rocks$mycol[rocks$bad.weather == 0] <- "purple"
```

Plot the data. Put `price` on the x-axis, and `quantity` on the y-axis (so that the data depict an "inverse" demand curve), and use `mycol` to determine colour:

```{r plot}
plot(x = rocks$price, y = rocks$quantity,
     main = "Price and Quantity of Earth Rocks",
     xlab = "Price",
     ylab = "Quantity",
     pch = 16,
     col = rocks$mycol)

legend("topright", 
       legend = c("bad weather", "good weather"), 
       col=c("orange", "purple"), pch=16)
```
![](https://rtgodwin.com/3040/images/plot-1.png)

# Estimate the effect of an increase in price on quantity

To estimate the model $quantity = \beta_0 + \beta_1price + \epsilon$, we can use the `lm()` function:

```r
lm(quantity ~ price, data = rocks)
```
```
Call:
lm(formula = quantity ~ price, data = rocks)

Coefficients:
(Intercept)        price  
    18.7532      -0.4266  
```

To get more information about the estimated model, such as the estimated standard errors and $R^2$, we need to put the `lm()` function inside the `summary()` function:

```r
summary(lm(quantity ~ price, data = rocks))
```

```r
    sum(myvector)

    ## [1] 20
```

The `sum()` function is looking for arguments that it can add together.
Put an object in the brackets, and the function will try to add. Try
`mean(myvector)`. You can get help on a function by typing `?` followed
by the function name, and running the command. For example, try
`?summary`.

# Logical operators

Logical operators are used to determine whether something is `TRUE` or
`FALSE`. Some logical operators are:

<div align="center">

<font size = "3">

<table>
<thead>
<tr class="header">
<th style="text-align: center;">Operator</th>
<th style="text-align: left;">Function</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: center;">&gt;</td>
<td style="text-align: left;">greater than</td>
</tr>
<tr class="even">
<td style="text-align: center;">==</td>
<td style="text-align: left;">equal to</td>
</tr>
<tr class="odd">
<td style="text-align: center;">&lt;</td>
<td style="text-align: left;">less than</td>
</tr>
<tr class="even">
<td style="text-align: center;">&gt;=</td>
<td style="text-align: left;">greater than or equal to</td>
</tr>
<tr class="odd">
<td style="text-align: center;">&lt;=</td>
<td style="text-align: left;">less than or equal to</td>
</tr>
<tr class="even">
<td style="text-align: center;">!=</td>
<td style="text-align: left;">not equal to</td>
</tr>
</tbody>
</table>

</font>
</div>

The operators `&` “and” and `|` “or” may be used to combine the above
operators.

Try entering the following commands:

```r
    8 > 4

    ## [1] TRUE

    b == 6

    ## [1] FALSE

    b > 2

    ## [1] TRUE

    myvector > 3

    ## [1] FALSE FALSE  TRUE  TRUE  TRUE

    myvector > 3 & myvector < 7

    ## [1] FALSE FALSE  TRUE  TRUE FALSE
```

For the last example, R has checked to see whether each element in
`myvector` is greater than 3 and less than 7.

We will use these logical operators to create *subsamples*. We can
choose certain observations in the data set based on values of the
variables.

# Load data

The data for this tutorial was scraped by [Abdulshaheed
Alqunber](https://www.kaggle.com/ashaheedq/video-games-sales-2019) and
is originally from (<https://www.vgchartz.com/>). We are using a sub
sample of the data, and only look at video game sales for games making
at least $100,000 USD in global sales.

The data is in the “comma-separated-format” or “.csv”. This is a very
simple and common format for storing data.

RStudio can read data from your computer, or from the internet. Load the
data with:

```r
    mydata <- read.csv("https://rtgodwin.com/data/vidsales.csv")
```

We have created a new object called `mydata`, and have assigned it the
values in the `.csv` file using the assignment operator `<-`. You could
have chosen a name different than `mydata`.

The `mydata` object shows up in the top-right of your screen. The sample
size is 4706 and there are 8 variables.

Click the “spreadsheet” icon next to the data set to view it. Take a
moment to look through the data, and make sure you have an idea of what
each of the variables are. The `Sales` variable is either the total
global sales of the game, or the sales from bundling with consoles, and
is measured in millions of US dollars. `Score` is the critic score from
video game reviewers, and is on a scale of 0 to 10, with 10 being best.
Notice that each video game (each observation in the data set) takes up
a different row, while the type of information on the video game (the
variables) takes up a different column. Close the “data” tab when you
are done viewing.

# Explore the data

Click the blue arrow next to `data` in the top-right of your screen.

<img src="https://rtgodwin.com/3040/images/pic7.png"
style="width:50.0%" />  
  
Each variable name is listed, along with some info. Notice that the
variables each have a type:

<div align="center">

<font size = "3">

<table>
<thead>
<tr class="header">
<th style="text-align: left;">Name</th>
<th style="text-align: left;">Type</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: left;">chr</td>
<td style="text-align: left;">A character string (words)</td>
</tr>
<tr class="even">
<td style="text-align: left;">num</td>
<td style="text-align: left;">Any real (continuous) number.</td>
</tr>
<tr class="odd">
<td style="text-align: left;">int</td>
<td style="text-align: left;">Any integer.</td>
</tr>
</tbody>
</table>

</font>
</div>

The “character” variables can be used to create dummy variables (more on
this later!)

To explore the data, we can calculate “summary” statistics for all the
variables:

```r
    summary(mydata)

    ##      Name              Genre           ESRB_Rating          Platform        
    ##  Length:4706        Length:4706        Length:4706        Length:4706       
    ##  Class :character   Class :character   Class :character   Class :character  
    ##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
    ##                                                                             
    ##                                                                             
    ##                                                                             
    ##   Publisher             Score           Sales             Year     
    ##  Length:4706        Min.   : 1.00   Min.   : 0.010   Min.   :1985  
    ##  Class :character   1st Qu.: 6.50   1st Qu.: 0.150   1st Qu.:2004  
    ##  Mode  :character   Median : 7.50   Median : 0.420   Median :2008  
    ##                     Mean   : 7.27   Mean   : 1.177   Mean   :2007  
    ##                     3rd Qu.: 8.30   3rd Qu.: 1.100   3rd Qu.:2010  
    ##                     Max.   :10.00   Max.   :82.860   Max.   :2020
```

The min, max, quartiles, and mean have been calculated. For example, the
sample mean `Score` is 7.27.

We can extract variables from the data set, and perform functions on
them. To calculate the sample variance for `Sales`:

```r
    var(mydata$Sales)

    ## [1] 7.607421
```

**Important:** when we type `mydata$Sales` we are getting the `Sales`
variable from within the `mydata` data set.

Now, calculate the correlation between `Sales` and `Score`:

```r
    cor(mydata$Sales, mydata$Score)

    ## [1] 0.2634555
```
What does this tell you?

## Sub-sampling

We can use the `subset()` function, together with logical operators, to
create sub-samples.

Nintendo is my favourite publisher. Let’s see their critic scores
vs. the critic scores from other publishers:

```r
    mean(subset(mydata$Score, mydata$Publisher == "Nintendo"))

    ## [1] 7.799296

    mean(subset(mydata$Score, mydata$Publisher != "Nintendo"))

    ## [1] 7.21722
```

Notice how we have used logical operators to create two sub-samples:
Nintendo games, and other games.

On your own, determine the total global sales in the data set for the
publisher “Rockstar Games”.

<span
class="spoiler">`sum(subset(mydata$Sales, mydata$Publisher == "Rockstar Games"))`</span>

Now, find all the video games that have received a perfect score of 10.

<span class="spoiler">`subset(mydata, mydata$Score == 10)`</span>

For the rest of the tutorial, we’ll use a sub-sample of all video games
that have sales 2 million USD or more:

```r
    vid2 <- subset(mydata, mydata$Sales >= 2)
```

The above line creates a new data set, by selecting only the rows from
`mydata` which have `Sales >= 2`. The new data set shows up in the
top-right panel. Check the sample size.

# Histograms and scatterplots

Visualization is important. Plot a histogram of critic scores:

```r
    hist(vid2$Score)
```

![](https://rtgodwin.com/3040/images/histo-1.png)

Many options are available to customize the histogram, see `?hist`.
Let’s add some labels, and control the number of “breakpoints”:

```r
    hist(vid2$Score, 
         main = "Histogram of video game critic scores", 
         xlab = "score", 
         breaks = 10)
```

![](https://rtgodwin.com/3040/images/histo2-1.png)

The scatterplot is a widely used tool for visualizing the relationship
between two variables. Draw a scatterplot for `Sales` and `Score`,
adding a title and labeling the axis:

```r
    plot(vid2$Score, vid2$Sales, 
         main = "critic scores and video game sales", 
         xlab = "score", ylab = "Sales")
```

![](https://rtgodwin.com/3040/images/scats2-1.png)

We can also change the color and style of the dots:

```r
    plot(vid2$Score, vid2$Sales, 
         col = 3, 
         pch = 16)
```

![](https://rtgodwin.com/3040/images/scats3-1.png)

Type `?pch` to see the different styles, and Google to see the different
colours.

# Create new variables

Let’s create a new variable that we’ll use to select the colour of each
data point. Make a new variable called `G` inside of the `vid2` dataset,
and use `G` to control the colour of each data point. We begin by
setting all rows of the variable equal to 1, and then change the value
of `G` based on the game’s `Genre`. Copy and paste the code below:

```r
    vid2$G <- 1
    vid2$G[vid2$Genre == "Action"] <- 2
    vid2$G[vid2$Genre == "Sports"] <- 3
    vid2$G[vid2$Genre == "Shooter"] <- 7
    vid2$G[vid2$Genre == "Role-Playing"] <- 4
    vid2$G[vid2$Genre == "Platform"] <- 5
    vid2$G[vid2$Genre == "Racing"] <- 6
```

For example, if `Genre` is equal to `"Action"`, the variable `G` gets a
value of 2. Look at this `G` variable in the spreadsheet to understand
what has happened, Now, use this variable to determine the colour for
each data point:

```r
    plot(vid2$Score, vid2$Sales, col=vid2$G, pch=16,
         main = "video game sales and scores by genre",
         xlab = "score", ylab = "sales")
```

![](https://rtgodwin.com/3040/images/scats4-1.png)

We need to add a legend to the plot to show what the colours mean:

```r
    plot(vid2$Score, vid2$Sales, col=vid2$G, pch=16)
    legend("topleft", 
           legend = c("Action", "Sports", "Shooter", "Role-Playing", "Platform", "Racing", "Other"), 
           col=c(2, 3, 7, 4, 5, 6, 1), pch=16)
```

![](https://rtgodwin.com/3040/images/leg-1.png)

# Import your work into Word

In the “plots” window, click “Export” and “Save as Image…”. This way,
you can import graphics you create into Word, or another word processor.

<img src="https://rtgodwin.com/3040/images/pic10.png"
style="width:50.0%" />

# Simple least-squares regression

What is the average increase in `Sales` associated with an increase in
`Score`? We can estimate a simple linear regression using the `lm()`
function (“lm” stands for “linear model”):

```r
    lm(Sales ~ Score, data = vid2)
    
    ##     Call:
    ## lm(formula = Sales ~ Score, data = vid2)
    ##
    ## Coefficients:
    ## (Intercept)        Score  
    ##     -2.1472       0.8912  
```

The interpretation of the results is that an increase in critic score of
1 is associated with an average increase in sales of $0.89 million.

# Save your script file

Make sure the top-left window is active, then click “File”, “Save As…”,
and name your script file.

# Clear your workspace

If you get too much junk floating around in R, you can clear the
environment by clicking the “sweep” icon in the top-right, and start
over.

<img src="https://rtgodwin.com/3040/images/pic9.png"
style="width:50.0%" />

  
  
