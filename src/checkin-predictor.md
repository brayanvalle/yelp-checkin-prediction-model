Prediction check-in model Yelp dataset
================

``` r
setwd("E:/Courses/R/yelp-checkin-prediction-model/src/")
```

Required libraries
==================

``` r
library(tidyverse)
```

    ## -- Attaching packages ----------------------------------------------------------------------------------------- tidyverse 1.2.1 --

    ## v ggplot2 3.1.1       v purrr   0.3.2  
    ## v tibble  2.1.1       v dplyr   0.8.0.1
    ## v tidyr   0.8.3       v stringr 1.4.0  
    ## v readr   1.3.1       v forcats 0.4.0

    ## -- Conflicts -------------------------------------------------------------------------------------------- tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
library(dplyr)
```

Read the dataset
================

``` r
fullData <- read.csv(file="../data/CheckinBusiness_DatosCompletos.csv", header=TRUE, sep=",")
```

View data
=========

``` r
head(fullData)
```

    ##   ï..rownumber            business_id      city stars review_count is_open
    ## 1            1 _1cBVJcvvxx3jxy6u5kSWQ Las Vegas     2           13       1
    ## 2            2 _1cBVJcvvxx3jxy6u5kSWQ Las Vegas     2           13       1
    ## 3            3 _1cBVJcvvxx3jxy6u5kSWQ Las Vegas     2           13       1
    ## 4            4 _1cBVJcvvxx3jxy6u5kSWQ Las Vegas     2           13       1
    ## 5            5 _1cBVJcvvxx3jxy6u5kSWQ Las Vegas     2           13       1
    ## 6            6 _1cBVJcvvxx3jxy6u5kSWQ Las Vegas     2           13       1
    ##        category day_time week_day hourofday checkins
    ## 1 Beauty & Spas    Fri-0      Fri         0        1
    ## 2 Beauty & Spas    Fri-1      Fri         1        0
    ## 3 Beauty & Spas   Fri-10      Fri        10        0
    ## 4 Beauty & Spas   Fri-11      Fri        11        0
    ## 5 Beauty & Spas   Fri-12      Fri        12        0
    ## 6 Beauty & Spas   Fri-13      Fri        13        0

Converting into tibble data frame
=================================

``` r
filteredData <- as_tibble(fullData)
filteredData
```

    ## # A tibble: 16,800 x 11
    ##    ï..rownumber business_id city  stars review_count is_open category
    ##           <int> <fct>       <fct> <dbl>        <int>   <int> <fct>   
    ##  1            1 _1cBVJcvvx~ Las ~     2           13       1 Beauty ~
    ##  2            2 _1cBVJcvvx~ Las ~     2           13       1 Beauty ~
    ##  3            3 _1cBVJcvvx~ Las ~     2           13       1 Beauty ~
    ##  4            4 _1cBVJcvvx~ Las ~     2           13       1 Beauty ~
    ##  5            5 _1cBVJcvvx~ Las ~     2           13       1 Beauty ~
    ##  6            6 _1cBVJcvvx~ Las ~     2           13       1 Beauty ~
    ##  7            7 _1cBVJcvvx~ Las ~     2           13       1 Beauty ~
    ##  8            8 _1cBVJcvvx~ Las ~     2           13       1 Beauty ~
    ##  9            9 _1cBVJcvvx~ Las ~     2           13       1 Beauty ~
    ## 10           10 _1cBVJcvvx~ Las ~     2           13       1 Beauty ~
    ## # ... with 16,790 more rows, and 4 more variables: day_time <fct>,
    ## #   week_day <fct>, hourofday <int>, checkins <int>

Selecting subset data for for all business in Phoenix city
==========================================================

``` r
filteredData <- filteredData %>% filter(city == "Phoenix")
```

Remove columns: \[Row number, city, day\_tile\]
===============================================

``` r
filteredData <- filteredData %>% select(stars , review_count , is_open , category ,week_day,hourofday,checkins)
filteredData
```

    ## # A tibble: 2,856 x 7
    ##    stars review_count is_open category         week_day hourofday checkins
    ##    <dbl>        <int>   <int> <fct>            <fct>        <int>    <int>
    ##  1     5            5       1 Health & Medical Fri              0        0
    ##  2     5            5       1 Health & Medical Fri              1        0
    ##  3     5            5       1 Health & Medical Fri             10        0
    ##  4     5            5       1 Health & Medical Fri             11        0
    ##  5     5            5       1 Health & Medical Fri             12        0
    ##  6     5            5       1 Health & Medical Fri             13        0
    ##  7     5            5       1 Health & Medical Fri             14        0
    ##  8     5            5       1 Health & Medical Fri             15        0
    ##  9     5            5       1 Health & Medical Fri             16        0
    ## 10     5            5       1 Health & Medical Fri             17        0
    ## # ... with 2,846 more rows

Recoding Week day to the asociative day of week
===============================================

``` r
filteredData$week_day <-recode(filteredData$week_day,'Sun'=1, 'Mon'=2, 'Tue'=3, 'Wed'=4 , 'Thu'=5, 'Fri'=6,'Sat'=7)
filteredData
```

    ## # A tibble: 2,856 x 7
    ##    stars review_count is_open category         week_day hourofday checkins
    ##    <dbl>        <int>   <int> <fct>               <dbl>     <int>    <int>
    ##  1     5            5       1 Health & Medical        6         0        0
    ##  2     5            5       1 Health & Medical        6         1        0
    ##  3     5            5       1 Health & Medical        6        10        0
    ##  4     5            5       1 Health & Medical        6        11        0
    ##  5     5            5       1 Health & Medical        6        12        0
    ##  6     5            5       1 Health & Medical        6        13        0
    ##  7     5            5       1 Health & Medical        6        14        0
    ##  8     5            5       1 Health & Medical        6        15        0
    ##  9     5            5       1 Health & Medical        6        16        0
    ## 10     5            5       1 Health & Medical        6        17        0
    ## # ... with 2,846 more rows

Select 70% for traning data
===========================

``` r
training_data_size <- ceiling(nrow(filteredData)*0.7)
training_data_size
```

    ## [1] 2000

Get training data
=================

``` r
training_data <- filteredData[0:training_data_size,]
```

Save training data to a csv file
================================

``` r
write.csv(training_data , file ="../data/traning_data.csv")
```

Get Test data
=============

``` r
test_data <- filteredData[(training_data_size+1):nrow(filteredData),]
```

Save Test data to a csv file
============================

``` r
write.csv(test_data , file ="../data/test_data.csv")
```
