# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data

To load the data, I am using read.csv function. Then data cleanup is performed:


```r
# read data
data <- read.csv("activity.csv")
# clean
data$date <- as.Date(data$date)
data$interval <- as.numeric(data$interval)
data$steps <- as.numeric(data$steps)
```

Now, I aggregte number of steps taken over dates and generate a histogram:


```r
sumByDate <- aggregate(steps ~ date, data, sum)
hist(sumByDate$steps, main="Histogram of sum of steps by date", xlab="sum of steps")
```

![](./PA1_template_files/figure-html/unnamed-chunk-1-1.png) 

## What is mean total number of steps taken per day?

To calculate the mean and median, following code is used:


```r
meanOfStepsTakenPerDay <- mean(sumByDate$steps)
medianOfStepsTakenPerDay <- median(sumByDate$steps)
```

This gives us the mean as **1.0766189\times 10^{4}** and medias as **1.0765\times 10^{4}**. (Note: inline notation was used to print the mean and median.)



## What is the average daily activity pattern?

Here is a plot for the Average Daily Activity Pattern:


```r
avgByInterval <- aggregate(steps ~ interval, data, mean)
plot(avgByInterval$interval, avgByInterval$steps, type="l", main="Average Daily Activity Pattern", xlab="Interval", ylab="Average steps")
```

![](./PA1_template_files/figure-html/unnamed-chunk-3-1.png) 

To compute the interval where maximum activity takes place, following is used:


```r
maxIndex <- which.max(avgByInterval$steps)
maxInterval <- avgByInterval[maxIndex, 1]
```
So the 5-minute interval, on average across all the days in the dataset, that contains the maximum number of steps is **835**

## Imputing missing values

The data set has **2304** missing values for steps.

The strategy used here for filling in all of the missing values in the dataset is to use the ```impute``` package and then uses its knn function to populate missing values:


```r
library(impute)
data2 <- data
dataMatrix <- data.matrix(data2)
dataMatrix <- impute.knn(dataMatrix)$data
data2$steps <- dataMatrix[,1]
sumByDate2 <- aggregate(steps ~ date, data2, sum)
```

The histogram of new data is:

```r
hist(sumByDate2$steps, main="Histogram of sum of steps by date", xlab="sum of steps")
```

![](./PA1_template_files/figure-html/unnamed-chunk-6-1.png) 

Calculating mean and median of new data:

```r
mean2OfStepsTakenPerDay <- mean(sumByDate2$steps)
median2OfStepsTakenPerDay <- median(sumByDate2$steps)
```

The new mean as **1.0002743\times 10^{4}** and new medias as **1.0395\times 10^{4}**. 

These values are less that the ones of the orignal data.

## Are there differences in activity patterns between weekdays and weekends?
