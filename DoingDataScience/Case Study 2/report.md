# Case Study II
Trace Smith & Damon Resnick  
November 21, 2016  




<br>

### Question 1

**Create the following X matrix and print it from SAS, R, and Python.**

![](https://udacity-github-sync-content.s3.amazonaws.com/_attachments/26272/1479788865/Screen_Shot_2016-11-21_at_10.27.27_PM.png)

- **SAS Code**

```{}
proc iml;
/*create 3x4 matrix*/
x={4 5 1 2,
   1 0 3 5,
   2 1 8 2};
run;
create mymatrix from x[colname={"a","b","c","d"}];
append from x;
close mymatrix;
proc print data=mymatrix;
run;
```

- SAS output for X matrix shown below:

![](https://udacity-github-sync-content.s3.amazonaws.com/_attachments/26272/1479789335/Screen_Shot_2016-11-21_at_10.35.22_PM.png)


- **R Code**


```r
mymatrix <- matrix(c(4, 1, 2, 5, 0, 1, 1, 3, 8, 2, 5, 2), nrow = 3, ncol = 4)
print(mymatrix)
```

```
##      [,1] [,2] [,3] [,4]
## [1,]    4    5    1    2
## [2,]    1    0    3    5
## [3,]    2    1    8    2
```

- **Python Code**

```{}
import numpy as np
x = np.matrix([[4,5,1,2],[1,0,3,5],[2,1,8,2]])
print x
```

 - Python output (Ipython Notebook):
 
 ![](https://udacity-github-sync-content.s3.amazonaws.com/_attachments/26272/1479789062/Screen_Shot_2016-11-21_at_10.30.42_PM.png)

<br>

### Question 2

- **Answer the following questions for Air Products & Chemicals, Inc. stock (symbol = `ADP`):**


- **1.) Download the data.**


- **2.) Calculate log returns.**


- **3.) Calculate volatility measure.**


- **4.) Calculate volatility over entire length of series for various three different decay factors.**


- **5.) Plot the results, overlaying the volatility curves on the data, just as was done in the S&P example.**

<br>

### Question 3

- The built-in data set called `Orange` in R is about the growth of orange trees. The `Orange` data frame has 3 columns of records of the growth of orange trees.

**Variable description**

- *Tree*: an ordered factor indicating the tree on which the measurement is made. The ordering  is according to increasing maximum diameter.

- *age*: a numeric vector giving the age of the tree (days since 1968/12/31) circumference: a numeric vector of trunk circumferences (mm). This is probably âcircumference at breast heightâ, a standard measurement in forestry.

- First, let's load the `Orange` data set into a data frame and examine the structure of the data:

```r
# Read in Orange dataset from R into data.frame
df <- data.frame(Orange)
```


```r
# Return first 5 rows of Orange df
head(df)
```

```
##   Tree  age circumference
## 1    1  118            30
## 2    1  484            58
## 3    1  664            87
## 4    1 1004           115
## 5    1 1231           120
## 6    1 1372           142
```


```r
# get summary of Orange dataset
summary(df)
```

```
##  Tree       age         circumference  
##  3:7   Min.   : 118.0   Min.   : 30.0  
##  1:7   1st Qu.: 484.0   1st Qu.: 65.5  
##  5:7   Median :1004.0   Median :115.0  
##  2:7   Mean   : 922.1   Mean   :115.9  
##  4:7   3rd Qu.:1372.0   3rd Qu.:161.5  
##        Max.   :1582.0   Max.   :214.0
```


```r
# get structure of each columns
str(df$Tree)
```

```
##  Ord.factor w/ 5 levels "3"<"1"<"5"<"2"<..: 2 2 2 2 2 2 2 4 4 4 ...
```

```r
str(df$age)
```

```
##  num [1:35] 118 484 664 1004 1231 ...
```

```r
str(df$circumference)
```

```
##  num [1:35] 30 58 87 115 120 142 145 33 69 111 ...
```

- **a) Calculate the mean and the median of the trunk circumferences for different size of the trees. (Tree)**


```r
# aggregate data.frame by Tree and compute mean circumference
circum.mean <- aggregate(df$circumference, by = list(df$Tree), FUN = mean)
colnames(circum.mean) <- c("Tree", "Avg Circumference")
circum.mean
```

```
##   Tree Avg Circumference
## 1    3          94.00000
## 2    1          99.57143
## 3    5         111.14286
## 4    2         135.28571
## 5    4         139.28571
```


```r
# aggregate data.frame by Tree and compute median circumference
circum.median <- aggregate(df$circumference, by = list(df$Tree), FUN = median)
colnames(circum.median) <- c("Tree", "Med. Circumference")
circum.median
```

```
##   Tree Med. Circumference
## 1    3                108
## 2    1                115
## 3    5                125
## 4    2                156
## 5    4                167
```

- **b) Make a scatter plot of the trunk circumferences against the age of the tree. Use different plotting symbols for different size of trees.**


```r
# Load ggplot2
library(ggplot2)

# Scatter plot
p <- ggplot(df) + geom_point(aes(y = df$circumference, x = df$age, colour = df$Tree)) + 
    scale_colour_hue(l = 80, c = 150)
p + labs(title = "Scatter Plot \n Age vs Circumference by Tree", x = "Age", y = "Circumference", 
    colour = "Tree")
```

<img src="report_files/figure-html/unnamed-chunk-8-1.png" width="1000px" />


```r
# Line plot
p <- ggplot(df, aes(y = df$circumference, x = df$age, colour = df$Tree)) + geom_point() + 
    geom_line(size = 1, alpha = 0.8) + scale_colour_hue(h = c(180, 270))
p + labs(title = "Line Plot \n Age vs Circumference by Tree", x = "Age", y = "Circumference", 
    colour = "Tree")
```

<img src="report_files/figure-html/unnamed-chunk-9-1.png" width="1000px" />


- **c) Display the trunk circumferences on a comparative boxplot against tree. Be sure you order the boxplots in the increasing order of maximum diameter.**


```r
# Determine the max circum by each group and reorder the levels accordingly
circum.max <- aggregate(df$circumference, by = list(df$Tree), FUN = max)  #aggregate for max circum
colnames(circum.max) <- c("Tree", "Max Circum.")  #rename columns
circum.max
```

```
##   Tree Max Circum.
## 1    3         140
## 2    1         145
## 3    5         177
## 4    2         203
## 5    4         214
```


```r
factor(df$Tree, c("3", "1", "5", "2", "4"))  #reorder the boxplot for max circum. by tree
```

```
##  [1] 1 1 1 1 1 1 1 2 2 2 2 2 2 2 3 3 3 3 3 3 3 4 4 4 4 4 4 4 5 5 5 5 5 5 5
## Levels: 3 < 1 < 5 < 2 < 4
```


```r
p <- ggplot(df, aes(x = Tree, y = circumference)) + geom_boxplot(aes(fill = Tree))  # ggplot: boxplot 
p + labs(title = "Box Plot: Trunk Circumference", y = "Circumference", x = "Tree")
```

<img src="report_files/figure-html/unnamed-chunk-12-1.png" width="1000px" />

<br>

### Question 4

- **1.)	First, d ownload âTempâ data set. Find the difference between the maximum and the minimum monthly average temperatures for each country and report/visualize top 20 countries with the maximum differences for the period since 1900.**


- **2.) Select a subset of data called âUStempâ where US land temperatures from 01/01/1990 in Temp data. Use UStemp dataset to answer the followings.**

  - **a) Create a new column to display the monthly average land temperatures in Fahrenheit (Â°F).**
  
  - **b) Calculate average land temperature by year and plot it. The original file has the average land temperature by month.** 
  
  - **c) Calculate the one year difference of average land temperature by year and provide the maximum difference (value) with corresponding two years.**


- **3.) Download âCityTempâ data set. Find the difference between the maximum and the minimum temperatures for each major city and report/visualize top 20 cities with maximum differences for the period since 1900.**

- **4.) Compare the two graphs in (i) and (iii)  and comment it.**







