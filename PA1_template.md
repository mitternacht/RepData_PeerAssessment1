# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data


```r
data_full <- read.csv("activity.csv")
```

## What is mean total number of steps taken per day?

Histogram of the total number of steps taken each day:


```r
data <- na.omit(data_full)
steps_per_day <- aggregate(steps ~ date, data, sum)
hist(steps_per_day$steps, main="Steps per Day", xlab="Steps", col=rgb(255, 37, 0, maxColorValue=255));
```

![](PA1_template_files/figure-html/unnamed-chunk-2-1.png) 

Mean of the total numbers of steps per day:


```r
mean(steps_per_day$steps)
```

```
## [1] 10766.19
```
        
Median of the total numbers of steps per day:


```r
median(steps_per_day$steps)
```

```
## [1] 10765
```

## What is the average daily activity pattern?

```r
avg_steps_per_interval <- aggregate(steps ~ interval, data, mean)
y <- avg_steps_per_interval$steps;
x <- avg_steps_per_interval$interval;
plot(x, y, type="l", ylab="Steps (average)", xlab="Interval");
```

![](PA1_template_files/figure-html/unnamed-chunk-5-1.png) 

5-minute interval (on average across all the days in the dataset) wich contains the maximum number of steps:


```r
max_index <- which.max(avg_steps_per_interval$steps)
avg_steps_per_interval[max_index,]$interval
```

```
## [1] 835
```

## Imputing missing values

Total number of missing values in the dataset: 


```r
nas <- data_full[is.na(data_full$steps), ]
nrow(nas)
```

```
## [1] 2304
```

Filling in all of the NAs with the average value of steps of each interval: 

```r
data_filled <- data_full
for (i in 1:nrow(data_filled))  {
        if(is.na(data_filled[i, ]$steps)){
                data_filled[i, ]$steps <- avg_steps_per_interval[avg_steps_per_interval$interval == data_filled[i, ]$interval, ]$steps
        }
}
```

Histogram of the total number of steps taken each day (using the dataset filled previously):


```r
steps_per_day_filled <- aggregate(steps ~ date, data_filled, sum)
hist(steps_per_day_filled$steps, main="Steps per Day (filled dataset)", xlab="Steps", col=rgb(255, 37, 0, maxColorValue=255));
```

![](PA1_template_files/figure-html/unnamed-chunk-9-1.png) 

Mean of the total numbers of steps per day (using the dataset filled previously):


```r
mean(steps_per_day_filled$steps)
```

```
## [1] 10766.19
```
        
Median of the total numbers of steps per day (using the dataset filled previously):


```r
median(steps_per_day_filled$steps)
```

```
## [1] 10766.19
```

```r
## Are there differences in activity patterns between weekdays and weekends?
```
