# PA1_template
KamalMehta  
15 July 2017  



## R Markdown

This is an R Markdown document. The document is created to specify the methods of reproducible research. The document identifies various steps and generates the output for the same  

**1. Code for reading in the dataset and/or processing the data**


```r
download.file("https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2Factivity.zip" , "Activity.zip")
unzip("Activity.zip")

dfHealth <- read.csv("activity.csv" , stringsAsFactors = FALSE)

str(dfHealth)
```

```
## 'data.frame':	17568 obs. of  3 variables:
##  $ steps   : int  NA NA NA NA NA NA NA NA NA NA ...
##  $ date    : chr  "2012-10-01" "2012-10-01" "2012-10-01" "2012-10-01" ...
##  $ interval: int  0 5 10 15 20 25 30 35 40 45 ...
```

```r
dfHealth$date <- as.Date(dfHealth$date , "%Y-%m-%d")
```

**2. Histogram of the total number of steps taken each day**


```r
dfStepsPerDay <- aggregate(dfHealth$steps , by = list(dfHealth$date) , FUN = sum)
hist(dfStepsPerDay$x , xlab = "No. of steps" , breaks = seq(0, 25000 ,by = 500) ,
     col = "lightblue" , main = "Number of steps per day")
```

![](PA1_template_files/figure-html/unnamed-chunk-2-1.png)<!-- -->

**3. Mean and median number of steps taken each day**


```r
summary(dfStepsPerDay$x)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
##      41    8841   10760   10770   13290   21190       8
```


**4 .Time series plot of the average number of steps taken**
This shows the plot keeping the NA values on specific days without hadling it to 0  


```r
plot(x = dfStepsPerDay$Group.1 , y = dfStepsPerDay$x , type = "l" , col = "green" ,
     xlab = "Days" , ylab = "No. of step" , lwd = 1, main = "Time series for steps without removing NAs")
```

![](PA1_template_files/figure-html/unnamed-chunk-4-1.png)<!-- -->


**5. The 5-minute interval that, on average, contains the maximum number of steps**


```r
dfStepsPerDay[dfStepsPerDay$x == max(dfStepsPerDay$x , na.rm = TRUE) & !is.na(dfStepsPerDay$x ) , ]
```

```
##       Group.1     x
## 54 2012-11-23 21194
```

```r
dfStepsPerInterval <- aggregate(dfHealth$steps , by  = list(dfHealth$interval) , FUN = mean , na.action = na.pass, na.rm = TRUE )

dfStepsPerInterval[dfStepsPerInterval$x == max(dfStepsPerInterval$x , na.rm = TRUE) 
                   & !is.na(dfStepsPerInterval$x ) , ]
```

```
##     Group.1        x
## 104     835 206.1698
```


**6. Code to describe and show a strategy for inputing missing data**


```r
dfStepsPerDay$x[is.na(dfStepsPerDay$x)] <- 0
```


**7. Histogram of the total number of steps taken each day after missing values are imputed**


```r
hist(dfStepsPerDay$x , xlab = "No. of steps per day" , breaks = seq(0, 25000 ,by = 500) ,
     col = "lightblue" , main = "Number of steps per day")
```

![](PA1_template_files/figure-html/unnamed-chunk-7-1.png)<!-- -->


**8. Panel plot comparing the average number of steps taken per 5-minute interval across weekdays and weekends**


```r
library(timeDate)

# filter the data
dfStepsDone <- dfHealth[!is.na(dfHealth$steps) , ]

# differentiate between weekday and weekend
dfStepsDoneWeekDay <- dfStepsDone[isWeekday(dfStepsDone$date) , ]
dfStepsDoneWeekEnd <- dfStepsDone[isWeekend(dfStepsDone$date) , ]

dfStepsDoneWeekDay <- aggregate(dfStepsDoneWeekDay$steps , by  = list(dfStepsDoneWeekDay$interval) , FUN = mean)
dfStepsDoneWeekEnd <- aggregate(dfStepsDoneWeekEnd$steps , by  = list(dfStepsDoneWeekEnd$interval) , FUN = mean)

plot(x = dfStepsDoneWeekDay$Group.1 , y = dfStepsDoneWeekDay$x , main = "Steps per interval on weekday"
     , type = "l" , ylab = "No. of steps"  , xlab = "Time Interval" , col = "orange")
```

![](PA1_template_files/figure-html/unnamed-chunk-8-1.png)<!-- -->

```r
plot(x = dfStepsDoneWeekEnd$Group.1 , y = dfStepsDoneWeekEnd$x , main = "Steps per interval on weekend"
     , type = "l"  , ylab = "No. of steps" , xlab = "Time Interval" , col = "green")
```

![](PA1_template_files/figure-html/unnamed-chunk-8-2.png)<!-- -->

