---
title: "Importing Data Frames"
date: "2022-01-11"
output:
  html_document: 
    theme: spacelab
    toc: yes
    toc_float: yes
    keep_md: yes
  pdf_document:
    toc: yes
---

## Breakout Rooms  
Please take 5-8 minutes to check over your answers to HW 2 in your group. If you are stuck, please remember that you can check the key in [Joel's repository](https://github.com/jmledford3115/BIS15LW2022_jledford).  

## Learning Goals
*At the end of this exercise, you will be able to:*    
1. Import .csv files as data frames using `read_csv()`.  
2. Use summary functions to explore the dimensions, structure, and contents of a data frame.  
3. Use the `select()` command of dplyr to sort data frames.  

## Review
At this point, you should have familiarity in RStudio, GitHub, and basic operations in R. You understand how to do arithmetic, assign values to objects, and work with vectors, data matrices, and data frames. If you are confused or need some extra help, please [email me](mailto: jmledford@ucdavis.edu).  

## Load the tidyverse

```r
library("tidyverse")
```

## Data Frames
For the remainder of the course, we will work exclusively with data frames. Recall that data frames store multiple classes of data. Last time, you were shown how to build data frames using the `data.frame()` command. However, scientists often make their data available as supplementary material associated with a publication. This is excellent scientific practice as it insures repeatability by showing exactly how analyses were performed. As data scientists, we capitalize on this by importing data directly into R.  

## Importing Data
R allows us to import a wide variety of data types. The most common type of file is a .csv file which stands for comma separated values. Spreadsheets are often developed in Excel then saved as .csv files for use in R. There are packages that allow you to open excel files and many other formats directly but .csv is the most common.  

An opinionated word about excel. It is fine to use excel for data entry and basic analysis. But, it often adds formatting that makes excel files difficult to work with in any program besides excel. R can read excel files, but I know of no R programmers that routinely use them. Instead they save copies of their excel files as .csv which strips away formatting but makes them easier to use in a variety of programs. We won't work with excel files in BIS 15L, but we will learn to import them.  

To import any file, first make sure that you are in the correct working directory. If you are not in the correct directory, R will not "see" the file.

```r
getwd()
```

```
## [1] "/Users/olijin/Documents/GitHub/BIS15W2022_QJin/lab3"
```

## Load the data
Here we open a .csv file. Since we are using the tidyverse, we open the file using `read_csv()`. `readr` is included in the tidyverse set of packages. We specify the package and function with the `::` symbol. This becomes important if you have multiple packages loaded that contain functions with the same name.  

In the previous part of the lab, you exported a `.csv` of hot springs data. Let's try to reload that `.csv`.  

```r
hot_springs <- readr::read_csv("hsprings_data.csv")
```

```
## Rows: 9 Columns: 4
```

```
## ?????? Column specification ????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????
## Delimiter: ","
## chr (2): scientist, spring
## dbl (2): temp_c, depth_ft
```

```
## 
## ??? Use `spec()` to retrieve the full column specification for this data.
## ??? Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

Use the `str()` function to get an idea of the data structure of `hot_springs`.  

```r
str(hot_springs)
```

```
## spec_tbl_df [9 ?? 4] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##  $ temp_c   : num [1:9] 36.2 35.4 35.3 35.1 35.4 ...
##  $ scientist: chr [1:9] "Jill" "Susan" "Steve" "Jill" ...
##  $ spring   : chr [1:9] "Buckeye" "Buckeye" "Buckeye" "Benton" ...
##  $ depth_ft : num [1:9] 4.15 4.13 4.12 3.21 3.23 3.2 5.67 5.65 5.66
##  - attr(*, "spec")=
##   .. cols(
##   ..   temp_c = col_double(),
##   ..   scientist = col_character(),
##   ..   spring = col_character(),
##   ..   depth_ft = col_double()
##   .. )
##  - attr(*, "problems")=<externalptr>
```

What is the class of the scientist column? Change it to factor and then show the levels of that factor.  

```r
class(hot_springs$scientist)
```

```
## [1] "character"
```


```r
hot_springs$scientist <- as.factor(hot_springs$scientist)
class(hot_springs$scientist)
```

```
## [1] "factor"
```


```r
levels(hot_springs$scientist)
```

```
## [1] "Jill"  "Steve" "Susan"
```

## Practice
1. In your lab 3 folder there is another folder titled `data`. Inside the `data` folder there is a `.csv` titled `Gaeta_etal_CLC_data.csv`. Open this data and store them as an object called `fish`.  

The data are from: Gaeta J., G. Sass, S. Carpenter. 2012. Biocomplexity at North Temperate Lakes LTER: Coordinated Field Studies: Large Mouth Bass Growth 2006. Environmental Data Initiative.  [link](https://portal.edirepository.org/nis/mapbrowse?scope=knb-lter-ntl&identifier=267)  


2. What is the structure of these data?


Notice that when the data are imported, you are presented with a message that tells you how R interpreted the column classes. This is also where error messages will appear if there are problems.  

## Summary functions
Once data have been uploaded, you may want to get an idea of its structure, contents, and dimensions. I routinely run one or more of these commands when data are first imported.  

We can summarize our data frame with the`summary()` function.  

```r
#summary(fish)
```

`glimpse()` is another useful summary function.

```r
#glimpse(fish)
```

`nrow()` gives the numbers of rows.

```r
#nrow(fish) #the number of rows or observations
```

`ncol` gives the number of columns.

```r
#ncol(fish) #the number of columns or variables
```

`dim()` gives the dimensions.

```r
#dim(fish) #total dimensions
```

`names` gives the column names.

```r
#names(fish) #column names
```

`head()` prints the first n rows of the data frame.

```r
#head(fish, n = 10)
```

`tail()` prinst the last n rows of the data frame.

```r
#tail(fish, n = 10)
```

`table()` is useful when you have a limited number of categorical variables. It produces fast counts of the number of observations in a variable. We will come back to this later... 

```r
#table(fish$lakeid)
```

We can also click on the `fish` data frame in the Environment tab or type View(fish).

```r
#View(fish)
```

## Subset
Subset is a way of pulling out observations that meet specific criteria in a variable.  

```r
#little_fish <- subset(fish, length<=100)
#little_fish
```

## Practice
1. Load the data `mammal_lifehistories_v2.csv` and place it into a new object called `mammals`.


2. Provide the dimensions of the data frame.


3. Check the column names in the data frame. 


4. Use `str()` to show the structure of the data frame and its individual columns; compare this to `glimpse()`. 




5. . Try the `table()` command to produce counts of mammal order, family, and genus.  


## Wrap-up
Please review the learning goals and be sure to use the code here as a reference when completing the homework.  
-->[Home](https://jmledford3115.github.io/datascibiol/)
