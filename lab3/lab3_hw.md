---
title: "Lab 3 Homework"
author: "Qianyu Jin"
date: "2022-01-13"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---

## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the tidyverse

```r
library(tidyverse)
```

## Mammals Sleep
1. For this assignment, we are going to use built-in data on mammal sleep patterns. From which publication are these data taken from? Since the data are built-in you can use the help function in R.

```r
readr::read_csv("data/mammals_sleep_allison_cicchetti_1976.csv")
```

```
## Rows: 62 Columns: 11
```

```
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr  (1): species
## dbl (10): body weight in kg, brain weight in g, slow wave ("nondreaming") sl...
```

```
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```
## # A tibble: 62 × 11
##    species  `body weight in … `brain weight i… `slow wave ("no… `paradoxical ("…
##    <chr>                <dbl>            <dbl>            <dbl>            <dbl>
##  1 African…          6654               5712             -999             -999  
##  2 African…             1                  6.6              6.3              2  
##  3 Arctic …             3.38              44.5           -999             -999  
##  4 Arctic …             0.92               5.7           -999             -999  
##  5 Asian e…          2547               4603                2.1              1.8
##  6 Baboon              10.6              180.               9.1              0.7
##  7 Big bro…             0.023              0.3             15.8              3.9
##  8 Brazili…           160                169                5.2              1  
##  9 Cat                  3.3               25.6             10.9              3.6
## 10 Chimpan…            52.2              440                8.3              1.4
## # … with 52 more rows, and 6 more variables:
## #   total sleep (hrs/day)  (sum of slow wave and paradoxical sleep) <dbl>,
## #   maximum life span (years) <dbl>, gestation time (days) <dbl>,
## #   predation index (1-5) <dbl>, sleep exposure index (1-5) <dbl>,
## #   overall danger index (1-5) <dbl>
```

2. Store these data into a new data frame `sleep`.

```r
sleep <- readr::read_csv("data/mammals_sleep_allison_cicchetti_1976.csv")
```

```
## Rows: 62 Columns: 11
```

```
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr  (1): species
## dbl (10): body weight in kg, brain weight in g, slow wave ("nondreaming") sl...
```

```
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

3. What are the dimensions of this data frame (variables and observations)? How do you know? Please show the *code* that you used to determine this below.  

This data fame contains 62 rows and 11 columns.

```r
dim(sleep)
```

```
## [1] 62 11
```

4. Are there any NAs in the data? How did you determine this? Please show your code. 

There are no NAs in the data.

```r
anyNA(sleep)
```

```
## [1] FALSE
```

5. Show a list of the column names is this data frame.

```r
names(sleep)
```

```
##  [1] "species"                                                        
##  [2] "body weight in kg"                                              
##  [3] "brain weight in g"                                              
##  [4] "slow wave (\"nondreaming\") sleep (hrs/day)"                    
##  [5] "paradoxical (\"dreaming\") sleep (hrs/day)"                     
##  [6] "total sleep (hrs/day)  (sum of slow wave and paradoxical sleep)"
##  [7] "maximum life span (years)"                                      
##  [8] "gestation time (days)"                                          
##  [9] "predation index (1-5)"                                          
## [10] "sleep exposure index (1-5)"                                     
## [11] "overall danger index (1-5)"
```

6. How many herbivores are represented in the data?  


7. We are interested in two groups; small and large mammals. Let's define small as less than or equal to 1kg body weight and large as greater than or equal to 200kg body weight. Make two new dataframes (large and small) based on these parameters.


8. What is the mean weight for both the small and large mammals?




9. Using a similar approach as above, do large or small animals sleep longer on average?  




10. Which animal is the sleepiest among the entire dataframe?




## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
