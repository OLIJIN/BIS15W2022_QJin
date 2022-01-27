---
title: "Midterm 1"
author: "Qianyu Jin"
date: "2022-01-26"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your code should be organized, clean, and run free from errors. Remember, you must remove the `#` for any included code chunks to run. Be sure to add your name to the author header above. You may use any resources to answer these questions (including each other), but you may not post questions to Open Stacks or external help sites. There are 15 total questions, each is worth 2 points.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

This exam is due by 12:00p on Thursday, January 27.  

## Load the tidyverse
If you plan to use any other libraries to complete this assignment then you should load them here.

```r
library(tidyverse)
```

```
## Warning: package 'tidyverse' was built under R version 4.0.2
```

```
## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──
```

```
## ✓ ggplot2 3.3.5     ✓ purrr   0.3.4
## ✓ tibble  3.1.6     ✓ dplyr   1.0.7
## ✓ tidyr   1.1.4     ✓ stringr 1.4.0
## ✓ readr   2.1.1     ✓ forcats 0.5.1
```

```
## Warning: package 'ggplot2' was built under R version 4.0.2
```

```
## Warning: package 'tibble' was built under R version 4.0.2
```

```
## Warning: package 'tidyr' was built under R version 4.0.2
```

```
## Warning: package 'readr' was built under R version 4.0.2
```

```
## Warning: package 'dplyr' was built under R version 4.0.2
```

```
## Warning: package 'forcats' was built under R version 4.0.2
```

```
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```r
library(janitor)
```

```
## Warning: package 'janitor' was built under R version 4.0.2
```

```
## 
## Attaching package: 'janitor'
```

```
## The following objects are masked from 'package:stats':
## 
##     chisq.test, fisher.test
```

## Questions  
Wikipedia's definition of [data science](https://en.wikipedia.org/wiki/Data_science): "Data science is an interdisciplinary field that uses scientific methods, processes, algorithms and systems to extract knowledge and insights from noisy, structured and unstructured data, and apply knowledge and actionable insights from data across a broad range of application domains."  

1. (2 points) Consider the definition of data science above. Although we are only part-way through the quarter, what specific elements of data science do you feel we have practiced? Provide at least one specific example.

**I think so far we have practiced the use of R studio's algorithms to rearrange as well as extract knowledge of interest, for example we could filter data with the 'filter()' function, which is used to subset rows using column values, extract rows which satisfy selected conditions. Then using functions such as 'mean()' or 'summary()' we could get other useful information from the selected datasets.**

2. (2 points) What is the most helpful or interesting thing you have learned so far in BIS 15L? What is something that you think needs more work or practice?  

**The most helpful thing I've learned so far is the ability to extract and manipulate data using existing functions, as well as the mindset that I am able to resolve problems using the R language, which is something that I wouldn't imagine to be able to do. I think I need more works on learning the easiest and functional way to combine functions, to not only solve problems, but find ways that are easily understandable and duplicatable for the future me or peers to use.**

In the midterm 1 folder there is a second folder called `data`. Inside the `data` folder, there is a .csv file called `ElephantsMF`. These data are from Phyllis Lee, Stirling University, and are related to Lee, P., et al. (2013), "Enduring consequences of early experiences: 40-year effects on survival and success among African elephants (Loxodonta africana)," Biology Letters, 9: 20130011. [kaggle](https://www.kaggle.com/mostafaelseidy/elephantsmf).  

3. (2 points) Please load these data as a new object called `elephants`. Use the function(s) of your choice to get an idea of the structure of the data. Be sure to show the class of each variable.

```r
elephants <- readr::read_csv("data/ElephantsMF.csv")
```

```
## Rows: 288 Columns: 3
```

```
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (1): Sex
## dbl (2): Age, Height
```

```
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```


4. (2 points) Change the names of the variables to lower case and change the class of the variable `sex` to a factor.

```r
elephants <- clean_names(elephants)
elephants$sex <- as.factor(elephants$sex)
elephants
```

```
## # A tibble: 288 × 3
##      age height sex  
##    <dbl>  <dbl> <fct>
##  1   1.4   120  M    
##  2  17.5   227  M    
##  3  12.8   235  M    
##  4  11.2   210  M    
##  5  12.7   220  M    
##  6  12.7   189  M    
##  7  12.2   225  M    
##  8  12.2   204  M    
##  9  28.2   266. M    
## 10  11.7   233  M    
## # … with 278 more rows
```


5. (2 points) How many male and female elephants are represented in the data?

**There are 150 female elephants and 138 male elephants in the data.**

```r
summary(elephants$sex)
```

```
##   F   M 
## 150 138
```


6. (2 points) What is the average age all elephants in the data?

**The average age of all elephants is 10.9713.**

```r
mean(elephants$age)
```

```
## [1] 10.97132
```


7. (2 points) How does the average age and height of elephants compare by sex?

**Comparing age, female elephants have a higher average of 12.835 than male elephants of 8.945. Comparing heights, female elephants have a taller average height of 190.031 than male elephants of 185.131.**

```r
male_elephants <- filter(elephants, sex=="M")
mean(male_elephants$age)
```

```
## [1] 8.945145
```

```r
mean(male_elephants$height)
```

```
## [1] 185.1312
```

```r
female_elephants <- filter(elephants,sex=="F")
mean(female_elephants$age)
```

```
## [1] 12.8354
```

```r
mean(female_elephants$height)
```

```
## [1] 190.0307
```


8. (2 points) How does the average height of elephants compare by sex for individuals over 20 years old. Include the min and max height as well as the number of individuals in the sample as part of your analysis.  

```r
maleover20 <- filter(elephants, age > 20 & sex=="M")
maleover20 %>%
  summarize(mean_height_maleover20 = mean(height), 
              min_height_maleover20 = min(height),
              max_height_maleover20 = max(height),
              total = n())
```

```
## # A tibble: 1 × 4
##   mean_height_maleover20 min_height_maleover20 max_height_maleover20 total
##                    <dbl>                 <dbl>                 <dbl> <int>
## 1                   270.                  229.                  304.    13
```

```r
femaleover20 <- filter(elephants, age > 20 & sex=="F")
femaleover20 %>%
  summarize(mean_height_femaleover20 = mean(height), 
              min_height_femaleover20 = min(height),
              max_height_femaleover20 = max(height),
              total = n())
```

```
## # A tibble: 1 × 4
##   mean_height_femaleover20 min_height_femaleover20 max_height_femaleover20 total
##                      <dbl>                   <dbl>                   <dbl> <int>
## 1                     232.                    193.                    278.    37
```

For the next series of questions, we will use data from a study on vertebrate community composition and impacts from defaunation in [Gabon, Africa](https://en.wikipedia.org/wiki/Gabon). One thing to notice is that the data include 24 separate transects. Each transect represents a path through different forest management areas.  

Reference: Koerner SE, Poulsen JR, Blanchard EJ, Okouyi J, Clark CJ. Vertebrate community composition and diversity declines along a defaunation gradient radiating from rural villages in Gabon. _Journal of Applied Ecology_. 2016. This paper, along with a description of the variables is included inside the midterm 1 folder.  


9. (2 points) Load `IvindoData_DryadVersion.csv` and use the function(s) of your choice to get an idea of the overall structure. Change the variables `HuntCat` and `LandUse` to factors.

```r
vertebrate <- readr::read_csv("data/IvindoData_DryadVersion.csv")
```

```
## Rows: 24 Columns: 26
```

```
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr  (2): HuntCat, LandUse
## dbl (24): TransectID, Distance, NumHouseholds, Veg_Rich, Veg_Stems, Veg_lian...
```

```
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
summary(vertebrate)
```

```
##    TransectID       Distance        HuntCat          NumHouseholds  
##  Min.   : 1.00   Min.   : 2.700   Length:24          Min.   :13.00  
##  1st Qu.: 5.75   1st Qu.: 5.668   Class :character   1st Qu.:24.75  
##  Median :14.50   Median : 9.720   Mode  :character   Median :29.00  
##  Mean   :13.50   Mean   :11.879                      Mean   :37.88  
##  3rd Qu.:20.25   3rd Qu.:17.683                      3rd Qu.:54.00  
##  Max.   :27.00   Max.   :26.760                      Max.   :73.00  
##    LandUse             Veg_Rich       Veg_Stems       Veg_liana     
##  Length:24          Min.   :10.88   Min.   :23.44   Min.   : 4.750  
##  Class :character   1st Qu.:13.10   1st Qu.:28.69   1st Qu.: 9.033  
##  Mode  :character   Median :14.94   Median :32.45   Median :11.940  
##                     Mean   :14.83   Mean   :32.80   Mean   :11.040  
##                     3rd Qu.:16.54   3rd Qu.:37.08   3rd Qu.:13.250  
##                     Max.   :18.75   Max.   :47.56   Max.   :16.380  
##     Veg_DBH        Veg_Canopy    Veg_Understory     RA_Apes      
##  Min.   :28.45   Min.   :2.500   Min.   :2.380   Min.   : 0.000  
##  1st Qu.:40.65   1st Qu.:3.250   1st Qu.:2.875   1st Qu.: 0.000  
##  Median :43.90   Median :3.430   Median :3.000   Median : 0.485  
##  Mean   :46.09   Mean   :3.469   Mean   :3.020   Mean   : 2.045  
##  3rd Qu.:50.58   3rd Qu.:3.750   3rd Qu.:3.167   3rd Qu.: 3.815  
##  Max.   :76.48   Max.   :4.000   Max.   :3.880   Max.   :12.930  
##     RA_Birds      RA_Elephant       RA_Monkeys      RA_Rodent    
##  Min.   :31.56   Min.   :0.0000   Min.   : 5.84   Min.   :1.060  
##  1st Qu.:52.51   1st Qu.:0.0000   1st Qu.:22.70   1st Qu.:2.047  
##  Median :57.90   Median :0.3600   Median :31.74   Median :3.230  
##  Mean   :58.64   Mean   :0.5450   Mean   :31.30   Mean   :3.278  
##  3rd Qu.:68.17   3rd Qu.:0.8925   3rd Qu.:39.88   3rd Qu.:4.093  
##  Max.   :85.03   Max.   :2.3000   Max.   :54.12   Max.   :6.310  
##   RA_Ungulate     Rich_AllSpecies Evenness_AllSpecies Diversity_AllSpecies
##  Min.   : 0.000   Min.   :15.00   Min.   :0.6680      Min.   :1.966       
##  1st Qu.: 1.232   1st Qu.:19.00   1st Qu.:0.7542      1st Qu.:2.248       
##  Median : 2.545   Median :20.00   Median :0.7760      Median :2.317       
##  Mean   : 4.166   Mean   :20.21   Mean   :0.7699      Mean   :2.310       
##  3rd Qu.: 5.157   3rd Qu.:22.00   3rd Qu.:0.8083      3rd Qu.:2.429       
##  Max.   :13.860   Max.   :24.00   Max.   :0.8330      Max.   :2.566       
##  Rich_BirdSpecies Evenness_BirdSpecies Diversity_BirdSpecies Rich_MammalSpecies
##  Min.   : 8.00    Min.   :0.5590       Min.   :1.162         Min.   : 6.000    
##  1st Qu.:10.00    1st Qu.:0.6825       1st Qu.:1.603         1st Qu.: 9.000    
##  Median :11.00    Median :0.7220       Median :1.680         Median :10.000    
##  Mean   :10.33    Mean   :0.7137       Mean   :1.661         Mean   : 9.875    
##  3rd Qu.:11.00    3rd Qu.:0.7722       3rd Qu.:1.784         3rd Qu.:11.000    
##  Max.   :13.00    Max.   :0.8240       Max.   :2.008         Max.   :12.000    
##  Evenness_MammalSpecies Diversity_MammalSpecies
##  Min.   :0.6190         Min.   :1.378          
##  1st Qu.:0.7073         1st Qu.:1.567          
##  Median :0.7390         Median :1.699          
##  Mean   :0.7477         Mean   :1.698          
##  3rd Qu.:0.7847         3rd Qu.:1.815          
##  Max.   :0.8610         Max.   :2.065
```

```r
vertebrate$HuntCat <- as.factor(vertebrate$HuntCat)
vertebrate$LandUse <- as.factor(vertebrate$LandUse)
```


10. (4 points) For the transects with high and moderate hunting intensity, how does the average diversity of birds and mammals compare?

**For transects with high hunting intensity, the average diversity of birds of 1.661 is lower than the average diversity of mammals of 1.737. For transects with moderate hunting intensity, the average diversity of birds of 1.621 is lower than the average diversity of mammals of 1.684.**

```r
highhunt <- filter(vertebrate,HuntCat=="High")
mean(highhunt$Diversity_BirdSpecies)
```

```
## [1] 1.660857
```

```r
mean(highhunt$Diversity_MammalSpecies)
```

```
## [1] 1.737
```

```r
moderatehunt <- filter(vertebrate,HuntCat=="Moderate")
mean(moderatehunt$Diversity_BirdSpecies)
```

```
## [1] 1.62125
```

```r
mean(moderatehunt$Diversity_MammalSpecies)
```

```
## [1] 1.68375
```


11. (4 points) One of the conclusions in the study is that the relative abundance of animals drops off the closer you get to a village. Let's try to reconstruct this (without the statistics). How does the relative abundance (RA) of apes, birds, elephants, monkeys, rodents, and ungulates compare between sites that are less than 3km from a village to sites that are greater than 25km from a village? The variable `Distance` measures the distance of the transect from the nearest village. Hint: try using the `across` operator.  

**The results show that the relative abundance of apes, monkeys, and ungulate drops at sites less than 3km from village. The reative abundance of birds, elephant, and rodent is greater at sites less than 3km from village.**

```r
distancelessthan3 <- filter(vertebrate,Distance<3)
distancelessthan3 %>%
  summarize(across(contains("RA"), mean))
```

```
## # A tibble: 1 × 7
##   TransectID RA_Apes RA_Birds RA_Elephant RA_Monkeys RA_Rodent RA_Ungulate
##        <dbl>   <dbl>    <dbl>       <dbl>      <dbl>     <dbl>       <dbl>
## 1         21    0.12     76.6       0.145       17.3      3.90        1.87
```

```r
distanceover25 <- filter(vertebrate,Distance>25)
distanceover25 %>%
  summarize(across(contains("RA"), mean))
```

```
## # A tibble: 1 × 7
##   TransectID RA_Apes RA_Birds RA_Elephant RA_Monkeys RA_Rodent RA_Ungulate
##        <dbl>   <dbl>    <dbl>       <dbl>      <dbl>     <dbl>       <dbl>
## 1         24    4.91     31.6           0       54.1      1.29        8.12
```


12. (4 points) Based on your interest, do one exploratory analysis on the `gabon` data of your choice. This analysis needs to include a minimum of two functions in `dplyr.`

**For this part, I tried to confirm if the richness of tree species varies based on the site distance to villages, and the relative abundance of apes and rodent. After extracting the average richness, average ape relative abundance, and average rodent relative abundance at distance over 15km and below 5km from villages. We observed that ,comparing with distance under 5km, the mean tree species richness is slightly higher with distance over 15km, and the corresponding relative abundance of apes is higher, with the relative abundance of rodent being lower.**

**In the article, we learned that apes as medium/large seed dispensers and rodent as seed predators, the relative abundance of each species also affect the abundance of tree species. It could be explained in the article as "The reduction of dispersal services and increase in seed predation could impede plant recruitment of primate-dispersed species, opening space for different plant species (Poulsen et al. 2002; Brodie et al. 2009)". Here with the extracted data below, it helped me to explore this phenomenon further.**

```r
vegetation <- select(vertebrate,"Distance","Veg_Rich","RA_Apes","RA_Rodent")
arrange(vegetation,desc(Distance))
```

```
## # A tibble: 24 × 4
##    Distance Veg_Rich RA_Apes RA_Rodent
##       <dbl>    <dbl>   <dbl>     <dbl>
##  1     26.8     16.8    4.91      1.29
##  2     24.1     14.8    3.78      3.1 
##  3     20.8     12.4   12.9       3.66
##  4     19.8     13.2    6.17      1.26
##  5     18.8     11.8    0.51      2.09
##  6     18.3     16.9    4.49      1.06
##  7     17.5     16.5    2.48      1.83
##  8     17.3     15.8    0         6.04
##  9     16.0     17.1    0         2.52
## 10     15.0     15      1.28      3.97
## # … with 14 more rows
```

```r
distover15 <- filter(vegetation, Distance>15)
distover15 %>%
  summarize(mean_dist = mean(Distance), 
              mean_veg_rich = mean(Veg_Rich),
              mean_RA_Apes = mean(RA_Apes),
              mean_RA_Rodent = mean(RA_Rodent),
              total = n())
```

```
## # A tibble: 1 × 5
##   mean_dist mean_veg_rich mean_RA_Apes mean_RA_Rodent total
##       <dbl>         <dbl>        <dbl>          <dbl> <int>
## 1      19.4          15.0         3.66           2.68    10
```

```r
distunder5 <- filter(vegetation, Distance<5)
distunder5 %>%
  summarize(mean_dist = mean(Distance), 
              mean_veg_rich = mean(Veg_Rich),
              mean_RA_Apes = mean(RA_Apes),
              mean_RA_Rodent = mean(RA_Rodent),
              total = n())
```

```
## # A tibble: 1 × 5
##   mean_dist mean_veg_rich mean_RA_Apes mean_RA_Rodent total
##       <dbl>         <dbl>        <dbl>          <dbl> <int>
## 1      3.15          12.5         0.08           3.66     3
```
