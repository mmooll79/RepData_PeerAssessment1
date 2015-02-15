Peer Assessment 1
=================

The following document is my answer for *Peer Assessment 1* during the course *Reproducible Research*. 

## Loading and preprocessing the data

As a first step let us load a dataset dedicated for this assessment and look for some details of it.


```r
activity <- read.csv("/Volumes/Obraz dysku/2015.02 Coursera/AS1/activity.csv")
activity$dateD <- as.Date(as.character(activity$date), "%Y-%m-%d")
summary(activity)
```

```
##      steps                date          interval          dateD           
##  Min.   :  0.00   2012-10-01:  288   Min.   :   0.0   Min.   :2012-10-01  
##  1st Qu.:  0.00   2012-10-02:  288   1st Qu.: 588.8   1st Qu.:2012-10-16  
##  Median :  0.00   2012-10-03:  288   Median :1177.5   Median :2012-10-31  
##  Mean   : 37.38   2012-10-04:  288   Mean   :1177.5   Mean   :2012-10-31  
##  3rd Qu.: 12.00   2012-10-05:  288   3rd Qu.:1766.2   3rd Qu.:2012-11-15  
##  Max.   :806.00   2012-10-06:  288   Max.   :2355.0   Max.   :2012-11-30  
##  NA's   :2304     (Other)   :15840
```

```r
str(activity)
```

```
## 'data.frame':	17568 obs. of  4 variables:
##  $ steps   : int  NA NA NA NA NA NA NA NA NA NA ...
##  $ date    : Factor w/ 61 levels "2012-10-01","2012-10-02",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ interval: int  0 5 10 15 20 25 30 35 40 45 ...
##  $ dateD   : Date, format: "2012-10-01" "2012-10-01" ...
```

## What is mean total number of steps taken per day?

### Calculate the total number of steps taken per day

I can use the fact that now in my dataset dates are stored as factors, so the total 
number of steps taken per each day is:

```r
StepsPerDay <- sapply(X = split(x = activity$steps, f = activity$date), 
                      FUN = function(x) sum(x, na.rm = TRUE))
print(StepsPerDay)
```

```
## 2012-10-01 2012-10-02 2012-10-03 2012-10-04 2012-10-05 2012-10-06 
##          0        126      11352      12116      13294      15420 
## 2012-10-07 2012-10-08 2012-10-09 2012-10-10 2012-10-11 2012-10-12 
##      11015          0      12811       9900      10304      17382 
## 2012-10-13 2012-10-14 2012-10-15 2012-10-16 2012-10-17 2012-10-18 
##      12426      15098      10139      15084      13452      10056 
## 2012-10-19 2012-10-20 2012-10-21 2012-10-22 2012-10-23 2012-10-24 
##      11829      10395       8821      13460       8918       8355 
## 2012-10-25 2012-10-26 2012-10-27 2012-10-28 2012-10-29 2012-10-30 
##       2492       6778      10119      11458       5018       9819 
## 2012-10-31 2012-11-01 2012-11-02 2012-11-03 2012-11-04 2012-11-05 
##      15414          0      10600      10571          0      10439 
## 2012-11-06 2012-11-07 2012-11-08 2012-11-09 2012-11-10 2012-11-11 
##       8334      12883       3219          0          0      12608 
## 2012-11-12 2012-11-13 2012-11-14 2012-11-15 2012-11-16 2012-11-17 
##      10765       7336          0         41       5441      14339 
## 2012-11-18 2012-11-19 2012-11-20 2012-11-21 2012-11-22 2012-11-23 
##      15110       8841       4472      12787      20427      21194 
## 2012-11-24 2012-11-25 2012-11-26 2012-11-27 2012-11-28 2012-11-29 
##      14478      11834      11162      13646      10183       7047 
## 2012-11-30 
##          0
```

### A histogram of the total number of steps taken each day


```r
hist(StepsPerDay)
```

![plot of chunk unnamed-chunk-3](figure/unnamed-chunk-3-1.png) 

### The mean and median of the total number of steps taken per day

The mean value of the total number of steps taken per day is:

```r
mean(StepsPerDay)
```

```
## [1] 9354.23
```

The median value of the total number of steps taken per day is:

```r
median(StepsPerDay)
```

```
## [1] 10395
```

## What is the average daily activity pattern?

A time series plot of the 5-minute interval (x-axis) and the average number of 
steps taken, averaged across all days (y-axis)


```r
x <- sapply(X = split(x = activity$steps, f = as.factor(activity$interval)), 
            FUN = function(x) mean(x, na.rm = TRUE))
plot(x = x, type = "l")
```

![plot of chunk unnamed-chunk-6](figure/unnamed-chunk-6-1.png) 

The 5-minute interval, which on average across all the days in the dataset, 
contains the maximum number of steps has a number


```r
names(x)[x==max(x)]
```

```
## [1] "835"
```


## Imputing missing values

The total number of missing values in the dataset is

```r
sum(!complete.cases(activity))
```

```
## [1] 2304
```

Imputing missing values I will use a mean value of steps for the 5-minute interval over all days.




## Are there differences in activity patterns between weekdays and weekends?

