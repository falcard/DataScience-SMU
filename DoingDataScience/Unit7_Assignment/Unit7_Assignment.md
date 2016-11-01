# Unit 7 - Live Assignment
Trace Smith  
October 26, 2016  



<br>

#### Question 1:
In base 7, the digits 0 to 6 are used. A number like 125 means 1 X 7^2 + 2 X 7 +5, just like base 10 with 10's replaced by 7's.


a.) When you count in base 7, you start 0,1,2,3,4,5,6,10,11,12,13,14,15,16,20, etc. Write a function in R called p7 (which takes ones argument n) that will print the first "n" numbers in base 7. 



b.) Write a function in R called base10to7 (which takes one argument x) that will convert a decimal number to base 7. To keep it simple, you can consider that x must be a scalar. For example, a decimal number 100 = 2 X 7^2 + 0 X 7^1 + 2 X 7^0 = 202 (base 7).


```r
base10to7<-function(x){
i=0
sum=0
while(x%/%7!=0){
sum<-sum+((x%%7)*(10^i))
i=i+1
x<-x%/%7
}
sum<-sum+((x%%7)*(10^i))
return(sum)
}
base10to7(100)
```

```
## [1] 202
```

c.) Write a function in R called base7to10 (which takes one argument y) that will convert a base 7 number to decimal. To keep it simple, consider that y must be a scalar.


```r
base7to10 <- function(y){
  sum = 0
  n = nchar(y)
  for (i in 1:n){
    p <- as.numeric(substr(y, (n-i+1), (n-i+1)))
    m<- p*(7^(i-1))
    sum <- sum+m
  }
  sum
}
```



```r
base7to10(202)
```

```
## [1] 100
```


d.) Can the functions you have written in a. - c. be generalized to base k(k=2,3...) instead of base 7? If yes, show how this can be done.



```r
base3to10 <- function(y){
  sum = 0
  n = nchar(y)
  for (i in 1:n){
    p <- as.numeric(substr(y, (n-i+1), (n-i+1)))
    m<- p*(3^(i-1))
    sum <- sum+m
  }
  sum
}
```


```r
base3to10(202)
```

```
## [1] 20
```