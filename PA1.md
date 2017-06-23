Reproducible Data Week 2
================
Wendy Williams
June 22, 2017

Introduction
============

It is now possible to collect a large amount of data about personal movement using activity monitoring devices such as a Fitbit, Nike Fuelband, or Jawbone Up. These type of devices are part of the "quantified self" movement - a group of enthusiasts who take measurements about themselves regularly to improve their health, to find patterns in their behavior, or because they are tech geeks. But these data remain under-utilized both because the raw data are hard to obtain and there is a lack of statistical methods and software for processing and interpreting the data.

This assignment makes use of data from a personal activity monitoring device. This device collects data at 5 minute intervals through out the day. The data consists of two months of data from an anonymous individual collected during the months of October and November, 2012 and include the number of steps taken in 5 minute intervals each day. The scripts for downloading, processing, and analyzing this data can be found below.

Downloading and Reading the Data
--------------------------------

1.  First set your working directory to the appropriate folder.
2.  Download the subset data file ("activity") into a folder called "Reproducible Research" from the url provided.

``` r
url <- "https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2Factivity.zip"
if(!file.exists("./Reproducible Research")) {dir.create("./Reproducible Research")}
download.file(url, destfile = "./Reproducible Research/repdata%2Fdata%2Factivity.zip")
```

1.  Set your working directory to "Reproducible Research", unzip the file ("activity.zip") and read the data into R using read.csv().

``` r
unzip("repdata%2Fdata%2Factivity.zip")
activity <- read.csv("activity.csv")
```

Review the Data
---------------

For a quick overview of the data file and guidance for analysis, use str(activity), summary(activity), and head(activity).

``` r
str(activity)
```

    ## 'data.frame':    17568 obs. of  3 variables:
    ##  $ steps   : int  NA NA NA NA NA NA NA NA NA NA ...
    ##  $ date    : Factor w/ 61 levels "2012-10-01","2012-10-02",..: 1 1 1 1 1 1 1 1 1 1 ...
    ##  $ interval: int  0 5 10 15 20 25 30 35 40 45 ...

``` r
summary(activity)
```

    ##      steps                date          interval     
    ##  Min.   :  0.00   2012-10-01:  288   Min.   :   0.0  
    ##  1st Qu.:  0.00   2012-10-02:  288   1st Qu.: 588.8  
    ##  Median :  0.00   2012-10-03:  288   Median :1177.5  
    ##  Mean   : 37.38   2012-10-04:  288   Mean   :1177.5  
    ##  3rd Qu.: 12.00   2012-10-05:  288   3rd Qu.:1766.2  
    ##  Max.   :806.00   2012-10-06:  288   Max.   :2355.0  
    ##  NA's   :2304     (Other)   :15840

``` r
head(activity)
```

    ##   steps       date interval
    ## 1    NA 2012-10-01        0
    ## 2    NA 2012-10-01        5
    ## 3    NA 2012-10-01       10
    ## 4    NA 2012-10-01       15
    ## 5    NA 2012-10-01       20
    ## 6    NA 2012-10-01       25

Process/Transform the Data
--------------------------

Convert the time data into a data class format.

``` r
activity$date <- as.Date(activity$date)
```

What is the mean total number of steps taken per day?
=====================================================

1.  Calculate the total number of steps taken each day, ignoring missing values.

``` r
eachday <- aggregate(activity$steps, by = list(activity$date), sum, na.rm = TRUE)
colnames(eachday) <- c("date", "steps")
eachday
```

    ##          date steps
    ## 1  2012-10-01     0
    ## 2  2012-10-02   126
    ## 3  2012-10-03 11352
    ## 4  2012-10-04 12116
    ## 5  2012-10-05 13294
    ## 6  2012-10-06 15420
    ## 7  2012-10-07 11015
    ## 8  2012-10-08     0
    ## 9  2012-10-09 12811
    ## 10 2012-10-10  9900
    ## 11 2012-10-11 10304
    ## 12 2012-10-12 17382
    ## 13 2012-10-13 12426
    ## 14 2012-10-14 15098
    ## 15 2012-10-15 10139
    ## 16 2012-10-16 15084
    ## 17 2012-10-17 13452
    ## 18 2012-10-18 10056
    ## 19 2012-10-19 11829
    ## 20 2012-10-20 10395
    ## 21 2012-10-21  8821
    ## 22 2012-10-22 13460
    ## 23 2012-10-23  8918
    ## 24 2012-10-24  8355
    ## 25 2012-10-25  2492
    ## 26 2012-10-26  6778
    ## 27 2012-10-27 10119
    ## 28 2012-10-28 11458
    ## 29 2012-10-29  5018
    ## 30 2012-10-30  9819
    ## 31 2012-10-31 15414
    ## 32 2012-11-01     0
    ## 33 2012-11-02 10600
    ## 34 2012-11-03 10571
    ## 35 2012-11-04     0
    ## 36 2012-11-05 10439
    ## 37 2012-11-06  8334
    ## 38 2012-11-07 12883
    ## 39 2012-11-08  3219
    ## 40 2012-11-09     0
    ## 41 2012-11-10     0
    ## 42 2012-11-11 12608
    ## 43 2012-11-12 10765
    ## 44 2012-11-13  7336
    ## 45 2012-11-14     0
    ## 46 2012-11-15    41
    ## 47 2012-11-16  5441
    ## 48 2012-11-17 14339
    ## 49 2012-11-18 15110
    ## 50 2012-11-19  8841
    ## 51 2012-11-20  4472
    ## 52 2012-11-21 12787
    ## 53 2012-11-22 20427
    ## 54 2012-11-23 21194
    ## 55 2012-11-24 14478
    ## 56 2012-11-25 11834
    ## 57 2012-11-26 11162
    ## 58 2012-11-27 13646
    ## 59 2012-11-28 10183
    ## 60 2012-11-29  7047
    ## 61 2012-11-30     0

1.  Plot a histogram of the total number of steps taken per day.

``` r
hist(eachday$steps, main = "Total Number of Steps Each Day", xlab = "Total Number of Steps in A Day")
```

![](PA1.template_files/figure-markdown_github-ascii_identifiers/unnamed-chunk-8-1.png)

1.  Calculate and report the mean and median number of steps taken each day, using summary on the data set "eachday".

``` r
summary(eachday$steps)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##       0    6778   10395    9354   12811   21194

The mean is 10766 and the median is 10765.

What is the average daily pattern?
==================================

1.  Make a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and average number of steps taken across all days (y-axis), ignoring missing values.

``` r
eachinterval = aggregate(activity$steps, by=list(interval = activity$interval), FUN=mean, na.rm = TRUE)
eachinterval$interval <- round(eachinterval$interval, 0)       
colnames(eachinterval) <- c("interval", "steps")

plot(eachinterval, type = "l", col = "blue", lwd = 2.5, ylab = "Number of Steps", font.lab = 2,
     main = "Time Series Plot for Average Daily Activity Pattern", xlab = "Interval" )
```

![](PA1.template_files/figure-markdown_github-ascii_identifiers/unnamed-chunk-10-1.png)

1.  Determine the 5-minute interval that, on average, contains the maximum number of steps.

Using data subsetted for the above time interval graph, averaging the number of steps for each interval, identify the interval with the max steps.

``` r
h = eachinterval$interval[which(eachinterval$steps == max(eachinterval$steps))]
h
```

    ## [1] 835

The interval with the maximum number of steps on average is 'h'.

Imputing missing values
=======================

Note that there are a number of days/intervals where there are missing values (coded as NA). The presence of missing days may introduce bias into some calculations or summaries of the data.

1.  Calculate and report the total number of missing values in the dataset (i.e. the total number of rows with NAs)

``` r
sum(is.na(activity$steps))
```

    ## [1] 2304

The total number of missing values is 2304.

1.  Devise a strategy for filling in all of the missing values in the dataset. The strategy does not need to be sophisticated. For example, you could use the mean/median for that day, or the mean for that 5-minute interval, etc.

For days that have no data,replace the NA values with the mean number of steps for the interval.

``` r
steps.average <- aggregate(steps ~ interval, data = activity, FUN = mean)
fillNA <- numeric()
for (i in 1:nrow(activity)) { obs <- activity[i, ]
    if (is.na(obs$steps)) {steps <- subset(steps.average, interval == obs$interval)$steps } 
    else {steps <- obs$steps}
    fillNA <- c(fillNA, steps) }
activity.fill <- cbind(activity[ , -1], fillNA)             # Create new dataset with imputed values filled
activity.fill$fillNA <- round(activity.fill$fillNA, 2); 
names(activity.fill)[3] <- "steps"
```

1.  Create a new dataset that is equal to the original dataset but with the missing data filled in.

``` r
sum(is.na(activity.fill$steps))
```

    ## [1] 0

1.  Make a histogram of the total number of steps taken each day and Calculate and report the mean and median total number of steps taken per day. Do these values differ from the estimates from the first part of the assignment? What is the impact of imputing missing data on the estimates of the total daily number of steps?

First, recalculate the number of steps per day using the imputed data set

``` r
fill.eachday  <- aggregate(activity.fill$steps, by = list(activity.fill$date), FUN = sum)
colnames(fill.eachday) <- c("date", "steps")
```

Plot a histogram of daily total number of steps taken, using a bin interval of 1000 steps

``` r
library(ggplot2)
ggplot(fill.eachday, aes(x = steps)) + 
    geom_histogram(fill = "blue", binwidth = 1000) + 
    labs(title = "Total Steps Taken per Day", x = "Steps", y = "Number of Days")
```

![](PA1.template_files/figure-markdown_github-ascii_identifiers/unnamed-chunk-16-1.png)

Calculate and report the mean and median number of steps taken each day, using summary on the data set "eachday".

``` r
summary(fill.eachday$steps)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##      41    9819   10766   10766   12811   21194

The resulting mean is 10766 and the median is 10766. Compared with the orginal values determined ignoring missing values (see below), the differences are minimal.

``` r
summary(eachday$steps)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##       0    6778   10395    9354   12811   21194

What is the impact of imputing missing data on the estimates of the total daily number of steps?

The median value has shifted by approximately 1 and the mean value remains unchanged.

Are there differences in activity patterns between weekdays and weekends?
=========================================================================

1.  Create a new factor variable in the dataset with two levels - "weekday" and "weekend" indicating whether a given date is a weekday or weekend day.

``` r
days <- weekdays(activity.fill$date)                                         # Extract day of the week
for(i in 1:length(days)) {                                                   # Reformat as binary 
        if(days[i] %in% c("Saturday", "Sunday")) days[i] = "Weekend"
        else days[i] = "Weekday" }
activity.fill$day <- as.factor(days)                                         # Attach to dataset
week.means <- aggregate(steps ~ interval + day, data = activity.fill, mean)  # Calculate means
```

1.  Make a panel plot containing a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all weekday days or weekend days (y-axis). See the README file in the GitHub repository to see an example of what this plot should look like using simulated data.

``` r
ggplot(week.means, aes(x = interval, y = steps)) + geom_line(color = "blue") + 
        facet_wrap( ~ day, nrow = 2, ncol = 1)   + labs(x = "Interval", y = "Number of steps")
```

![](PA1.template_files/figure-markdown_github-ascii_identifiers/unnamed-chunk-20-1.png)

According to the plots, weekdays are marked by the highest number of steps per interval followed by relatively low activity (number of steps), presumeably sitting at a desk. Weekends however are marked by more consistent activity (number of steps)
