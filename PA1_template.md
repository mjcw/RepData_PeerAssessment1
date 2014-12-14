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

![](./PA1_template_files/figure-html/unnamed-chunk-2-1.png) 

## What is mean total number of steps taken per day?

To calculate the mean and median, following code is used:


```r
meanOfStepsTakenPerDay <- mean(sumByDate$steps)
medianOfStepsTakenPerDay <- median(sumByDate$steps)
```

This gives us the mean as 1.0766189\times 10^{4} and medias as 1.0765\times 10^{4}. (Note: inline notation was used to print the mean and median.)



## What is the average daily activity pattern?


```r
avgByInterval <- aggregate(steps ~ interval, data, mean)
plot(avgByInterval$interval, avgByInterval$steps, type="l", main="Daily Activity Pattern", xlab="Interval", ylab="Average steps")
```

![](./PA1_template_files/figure-html/unnamed-chunk-4-1.png) 


## Imputing missing values



## Are there differences in activity patterns between weekdays and weekends?
