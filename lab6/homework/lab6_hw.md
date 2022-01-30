---
title: "Lab 6 Homework"
author: "Qianyu Jin"
date: "2022-01-29"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the libraries

```r
library(tidyverse)
library(janitor)
library(skimr)
```

For this assignment we are going to work with a large data set from the [United Nations Food and Agriculture Organization](http://www.fao.org/about/en/) on world fisheries. These data are pretty wild, so we need to do some cleaning. First, load the data.  

Load the data `FAO_1950to2012_111914.csv` as a new object titled `fisheries`.

```r
fisheries <- readr::read_csv(file = "data/FAO_1950to2012_111914.csv")
```

```
## Rows: 17692 Columns: 71
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (69): Country, Common name, ISSCAAP taxonomic group, ASFIS species#, ASF...
## dbl  (2): ISSCAAP group#, FAO major fishing area
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

1. Do an exploratory analysis of the data (your choice). What are the names of the variables, what are the dimensions, are there any NA's, what are the classes of the variables?  

```r
names(fisheries)
```

```
##  [1] "Country"                 "Common name"            
##  [3] "ISSCAAP group#"          "ISSCAAP taxonomic group"
##  [5] "ASFIS species#"          "ASFIS species name"     
##  [7] "FAO major fishing area"  "Measure"                
##  [9] "1950"                    "1951"                   
## [11] "1952"                    "1953"                   
## [13] "1954"                    "1955"                   
## [15] "1956"                    "1957"                   
## [17] "1958"                    "1959"                   
## [19] "1960"                    "1961"                   
## [21] "1962"                    "1963"                   
## [23] "1964"                    "1965"                   
## [25] "1966"                    "1967"                   
## [27] "1968"                    "1969"                   
## [29] "1970"                    "1971"                   
## [31] "1972"                    "1973"                   
## [33] "1974"                    "1975"                   
## [35] "1976"                    "1977"                   
## [37] "1978"                    "1979"                   
## [39] "1980"                    "1981"                   
## [41] "1982"                    "1983"                   
## [43] "1984"                    "1985"                   
## [45] "1986"                    "1987"                   
## [47] "1988"                    "1989"                   
## [49] "1990"                    "1991"                   
## [51] "1992"                    "1993"                   
## [53] "1994"                    "1995"                   
## [55] "1996"                    "1997"                   
## [57] "1998"                    "1999"                   
## [59] "2000"                    "2001"                   
## [61] "2002"                    "2003"                   
## [63] "2004"                    "2005"                   
## [65] "2006"                    "2007"                   
## [67] "2008"                    "2009"                   
## [69] "2010"                    "2011"                   
## [71] "2012"
```



```r
dim(fisheries)
```

```
## [1] 17692    71
```



```r
anyNA(fisheries)
```

```
## [1] TRUE
```



```r
summary(fisheries)
```

```
##    Country          Common name        ISSCAAP group#  ISSCAAP taxonomic group
##  Length:17692       Length:17692       Min.   :11.00   Length:17692           
##  Class :character   Class :character   1st Qu.:33.00   Class :character       
##  Mode  :character   Mode  :character   Median :36.00   Mode  :character       
##                                        Mean   :37.38                          
##                                        3rd Qu.:38.00                          
##                                        Max.   :77.00                          
##  ASFIS species#     ASFIS species name FAO major fishing area
##  Length:17692       Length:17692       Min.   :18.00         
##  Class :character   Class :character   1st Qu.:31.00         
##  Mode  :character   Mode  :character   Median :37.00         
##                                        Mean   :45.34         
##                                        3rd Qu.:57.00         
##                                        Max.   :88.00         
##    Measure              1950               1951               1952          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1953               1954               1955               1956          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1957               1958               1959               1960          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1961               1962               1963               1964          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1965               1966               1967               1968          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1969               1970               1971               1972          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1973               1974               1975               1976          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1977               1978               1979               1980          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1981               1982               1983               1984          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1985               1986               1987               1988          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1989               1990               1991               1992          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1993               1994               1995               1996          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1997               1998               1999               2000          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      2001               2002               2003               2004          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      2005               2006               2007               2008          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      2009               2010               2011               2012          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
## 
```

2. Use `janitor` to rename the columns and make them easier to use. As part of this cleaning step, change `country`, `isscaap_group_number`, `asfis_species_number`, and `fao_major_fishing_area` to data class factor. 

```r
fisheries <- clean_names(fisheries)
fisheries
```

```
## # A tibble: 17,692 × 71
##    country common_name        isscaap_group_n… isscaap_taxonom… asfis_species_n…
##    <chr>   <chr>                         <dbl> <chr>            <chr>           
##  1 Albania Angelsharks, sand…               38 Sharks, rays, c… 10903XXXXX      
##  2 Albania Atlantic bonito                  36 Tunas, bonitos,… 1750100101      
##  3 Albania Barracudas nei                   37 Miscellaneous p… 17710001XX      
##  4 Albania Blue and red shri…               45 Shrimps, prawns  2280203101      
##  5 Albania Blue whiting(=Pou…               32 Cods, hakes, ha… 1480403301      
##  6 Albania Bluefish                         37 Miscellaneous p… 1702021301      
##  7 Albania Bogue                            33 Miscellaneous c… 1703926101      
##  8 Albania Caramote prawn                   45 Shrimps, prawns  2280100117      
##  9 Albania Catsharks, nurseh…               38 Sharks, rays, c… 10801003XX      
## 10 Albania Common cuttlefish                57 Squids, cuttlef… 3210200202      
## # … with 17,682 more rows, and 66 more variables: asfis_species_name <chr>,
## #   fao_major_fishing_area <dbl>, measure <chr>, x1950 <chr>, x1951 <chr>,
## #   x1952 <chr>, x1953 <chr>, x1954 <chr>, x1955 <chr>, x1956 <chr>,
## #   x1957 <chr>, x1958 <chr>, x1959 <chr>, x1960 <chr>, x1961 <chr>,
## #   x1962 <chr>, x1963 <chr>, x1964 <chr>, x1965 <chr>, x1966 <chr>,
## #   x1967 <chr>, x1968 <chr>, x1969 <chr>, x1970 <chr>, x1971 <chr>,
## #   x1972 <chr>, x1973 <chr>, x1974 <chr>, x1975 <chr>, x1976 <chr>, …
```


```r
fisheries$country <- as.factor(fisheries$country)
fisheries$isscaap_group_number <- as.factor(fisheries$isscaap_group_number)
fisheries$asfis_species_number <- as.factor(fisheries$asfis_species_number)
fisheries$fao_major_fishing_area <- as.factor(fisheries$fao_major_fishing_area)
summary(fisheries)
```

```
##                      country      common_name        isscaap_group_number
##  Spain                   :  915   Length:17692       33     :3120        
##  Portugal                :  763   Class :character   36     :3051        
##  United States of America:  627   Mode  :character   37     :1838        
##  Korea, Republic of      :  620                      34     :1806        
##  Japan                   :  611                      38     :1608        
##  Russian Federation      :  523                      32     :1120        
##  (Other)                 :13633                      (Other):5149        
##  isscaap_taxonomic_group    asfis_species_number asfis_species_name
##  Length:17692            199XXXXXXX010:  446     Length:17692      
##  Class :character        1750102610   :  288     Class :character  
##  Mode  :character        175XXXXXXX   :  270     Mode  :character  
##                          199XXXXXXX054:  269                       
##                          1750102612   :  264                       
##                          1750102501   :  253                       
##                          (Other)      :15902                       
##  fao_major_fishing_area   measure             x1950          
##  34     :2650           Length:17692       Length:17692      
##  27     :2421           Class :character   Class :character  
##  37     :1707           Mode  :character   Mode  :character  
##  51     :1676                                                
##  31     :1221                                                
##  47     :1072                                                
##  (Other):6945                                                
##     x1951              x1952              x1953              x1954          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##     x1955              x1956              x1957              x1958          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##     x1959              x1960              x1961              x1962          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##     x1963              x1964              x1965              x1966          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##     x1967              x1968              x1969              x1970          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##     x1971              x1972              x1973              x1974          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##     x1975              x1976              x1977              x1978          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##     x1979              x1980              x1981              x1982          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##     x1983              x1984              x1985              x1986          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##     x1987              x1988              x1989              x1990          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##     x1991              x1992              x1993              x1994          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##     x1995              x1996              x1997              x1998          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##     x1999              x2000              x2001              x2002          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##     x2003              x2004              x2005              x2006          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##     x2007              x2008              x2009              x2010          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##     x2011              x2012          
##  Length:17692       Length:17692      
##  Class :character   Class :character  
##  Mode  :character   Mode  :character  
##                                       
##                                       
##                                       
## 
```

We need to deal with the years because they are being treated as characters and start with an X. We also have the problem that the column names that are years actually represent data. We haven't discussed tidy data yet, so here is some help. You should run this ugly chunk to transform the data for the rest of the homework. It will only work if you have used janitor to rename the variables in question 2!

```r
fisheries_tidy <- fisheries %>% 
  pivot_longer(-c(country,common_name,isscaap_group_number,isscaap_taxonomic_group,asfis_species_number,asfis_species_name,fao_major_fishing_area,measure),
              names_to = "year",
               values_to = "catch",
               values_drop_na = TRUE) %>% 
  mutate(year= as.numeric(str_replace(year, 'x', ''))) %>% 
  mutate(catch= str_replace(catch, c(' F'), replacement = '')) %>% 
  mutate(catch= str_replace(catch, c('...'), replacement = '')) %>% 
  mutate(catch= str_replace(catch, c('-'), replacement = '')) %>% 
  mutate(catch= str_replace(catch, c('0 0'), replacement = ''))

fisheries_tidy$catch <- as.numeric(fisheries_tidy$catch)
```

3. How many countries are represented in the data? Provide a count and list their names.

```r
?nlevels
nlevels(fisheries$country)
```

```
## [1] 204
```

```r
levels(fisheries$country)
```

```
##   [1] "Albania"                   "Algeria"                  
##   [3] "American Samoa"            "Angola"                   
##   [5] "Anguilla"                  "Antigua and Barbuda"      
##   [7] "Argentina"                 "Aruba"                    
##   [9] "Australia"                 "Bahamas"                  
##  [11] "Bahrain"                   "Bangladesh"               
##  [13] "Barbados"                  "Belgium"                  
##  [15] "Belize"                    "Benin"                    
##  [17] "Bermuda"                   "Bonaire/S.Eustatius/Saba" 
##  [19] "Bosnia and Herzegovina"    "Brazil"                   
##  [21] "British Indian Ocean Ter"  "British Virgin Islands"   
##  [23] "Brunei Darussalam"         "Bulgaria"                 
##  [25] "Cabo Verde"                "Cambodia"                 
##  [27] "Cameroon"                  "Canada"                   
##  [29] "Cayman Islands"            "Channel Islands"          
##  [31] "Chile"                     "China"                    
##  [33] "China, Hong Kong SAR"      "China, Macao SAR"         
##  [35] "Colombia"                  "Comoros"                  
##  [37] "Congo, Dem. Rep. of the"   "Congo, Republic of"       
##  [39] "Cook Islands"              "Costa Rica"               
##  [41] "Croatia"                   "Cuba"                     
##  [43] "Cura\xe7ao"                "Cyprus"                   
##  [45] "C\xf4te d'Ivoire"          "Denmark"                  
##  [47] "Djibouti"                  "Dominica"                 
##  [49] "Dominican Republic"        "Ecuador"                  
##  [51] "Egypt"                     "El Salvador"              
##  [53] "Equatorial Guinea"         "Eritrea"                  
##  [55] "Estonia"                   "Ethiopia"                 
##  [57] "Falkland Is.(Malvinas)"    "Faroe Islands"            
##  [59] "Fiji, Republic of"         "Finland"                  
##  [61] "France"                    "French Guiana"            
##  [63] "French Polynesia"          "French Southern Terr"     
##  [65] "Gabon"                     "Gambia"                   
##  [67] "Georgia"                   "Germany"                  
##  [69] "Ghana"                     "Gibraltar"                
##  [71] "Greece"                    "Greenland"                
##  [73] "Grenada"                   "Guadeloupe"               
##  [75] "Guam"                      "Guatemala"                
##  [77] "Guinea"                    "GuineaBissau"             
##  [79] "Guyana"                    "Haiti"                    
##  [81] "Honduras"                  "Iceland"                  
##  [83] "India"                     "Indonesia"                
##  [85] "Iran (Islamic Rep. of)"    "Iraq"                     
##  [87] "Ireland"                   "Isle of Man"              
##  [89] "Israel"                    "Italy"                    
##  [91] "Jamaica"                   "Japan"                    
##  [93] "Jordan"                    "Kenya"                    
##  [95] "Kiribati"                  "Korea, Dem. People's Rep" 
##  [97] "Korea, Republic of"        "Kuwait"                   
##  [99] "Latvia"                    "Lebanon"                  
## [101] "Liberia"                   "Libya"                    
## [103] "Lithuania"                 "Madagascar"               
## [105] "Malaysia"                  "Maldives"                 
## [107] "Malta"                     "Marshall Islands"         
## [109] "Martinique"                "Mauritania"               
## [111] "Mauritius"                 "Mayotte"                  
## [113] "Mexico"                    "Micronesia, Fed.States of"
## [115] "Monaco"                    "Montenegro"               
## [117] "Montserrat"                "Morocco"                  
## [119] "Mozambique"                "Myanmar"                  
## [121] "Namibia"                   "Nauru"                    
## [123] "Netherlands"               "Netherlands Antilles"     
## [125] "New Caledonia"             "New Zealand"              
## [127] "Nicaragua"                 "Nigeria"                  
## [129] "Niue"                      "Norfolk Island"           
## [131] "Northern Mariana Is."      "Norway"                   
## [133] "Oman"                      "Other nei"                
## [135] "Pakistan"                  "Palau"                    
## [137] "Palestine, Occupied Tr."   "Panama"                   
## [139] "Papua New Guinea"          "Peru"                     
## [141] "Philippines"               "Pitcairn Islands"         
## [143] "Poland"                    "Portugal"                 
## [145] "Puerto Rico"               "Qatar"                    
## [147] "Romania"                   "Russian Federation"       
## [149] "R\xe9union"                "Saint Barth\xe9lemy"      
## [151] "Saint Helena"              "Saint Kitts and Nevis"    
## [153] "Saint Lucia"               "Saint Vincent/Grenadines" 
## [155] "SaintMartin"               "Samoa"                    
## [157] "Sao Tome and Principe"     "Saudi Arabia"             
## [159] "Senegal"                   "Serbia and Montenegro"    
## [161] "Seychelles"                "Sierra Leone"             
## [163] "Singapore"                 "Sint Maarten"             
## [165] "Slovenia"                  "Solomon Islands"          
## [167] "Somalia"                   "South Africa"             
## [169] "Spain"                     "Sri Lanka"                
## [171] "St. Pierre and Miquelon"   "Sudan"                    
## [173] "Sudan (former)"            "Suriname"                 
## [175] "Svalbard and Jan Mayen"    "Sweden"                   
## [177] "Syrian Arab Republic"      "Taiwan Province of China" 
## [179] "Tanzania, United Rep. of"  "Thailand"                 
## [181] "TimorLeste"                "Togo"                     
## [183] "Tokelau"                   "Tonga"                    
## [185] "Trinidad and Tobago"       "Tunisia"                  
## [187] "Turkey"                    "Turks and Caicos Is."     
## [189] "Tuvalu"                    "Ukraine"                  
## [191] "Un. Sov. Soc. Rep."        "United Arab Emirates"     
## [193] "United Kingdom"            "United States of America" 
## [195] "Uruguay"                   "US Virgin Islands"        
## [197] "Vanuatu"                   "Venezuela, Boliv Rep of"  
## [199] "Viet Nam"                  "Wallis and Futuna Is."    
## [201] "Western Sahara"            "Yemen"                    
## [203] "Yugoslavia SFR"            "Zanzibar"
```

4. Refocus the data only to include country, isscaap_taxonomic_group, asfis_species_name, asfis_species_number, year, catch.

```r
select(fisheries_tidy,-'common_name',-'isscaap_group_number',-'fao_major_fishing_area',-'measure')
```

```
## # A tibble: 376,771 × 6
##    country isscaap_taxonomic_group asfis_species_n… asfis_species_n…  year catch
##    <fct>   <chr>                   <fct>            <chr>            <dbl> <dbl>
##  1 Albania Sharks, rays, chimaeras 10903XXXXX       Squatinidae       1995    NA
##  2 Albania Sharks, rays, chimaeras 10903XXXXX       Squatinidae       1996    53
##  3 Albania Sharks, rays, chimaeras 10903XXXXX       Squatinidae       1997    20
##  4 Albania Sharks, rays, chimaeras 10903XXXXX       Squatinidae       1998    31
##  5 Albania Sharks, rays, chimaeras 10903XXXXX       Squatinidae       1999    30
##  6 Albania Sharks, rays, chimaeras 10903XXXXX       Squatinidae       2000    30
##  7 Albania Sharks, rays, chimaeras 10903XXXXX       Squatinidae       2001    16
##  8 Albania Sharks, rays, chimaeras 10903XXXXX       Squatinidae       2002    79
##  9 Albania Sharks, rays, chimaeras 10903XXXXX       Squatinidae       2003     1
## 10 Albania Sharks, rays, chimaeras 10903XXXXX       Squatinidae       2004     4
## # … with 376,761 more rows
```

5. Based on the asfis_species_number, how many distinct fish species were caught as part of these data?

```r
nlevels(fisheries_tidy$asfis_species_number)
```

```
## [1] 1553
```

6. Which country had the largest overall catch in the year 2000?
China had the largest overall catch in year 2000.

```r
fisheries_tidy %>%
  select('country','year','catch')%>%
  filter(year==2000)%>%
  arrange(desc(catch))
```

```
## # A tibble: 8,793 × 3
##    country                   year catch
##    <fct>                    <dbl> <dbl>
##  1 China                     2000  9068
##  2 Peru                      2000  5717
##  3 Russian Federation        2000  5065
##  4 Viet Nam                  2000  4945
##  5 Chile                     2000  4299
##  6 China                     2000  3288
##  7 China                     2000  2782
##  8 United States of America  2000  2438
##  9 China                     2000  1234
## 10 Philippines               2000   999
## # … with 8,783 more rows
```

7. Which country caught the most sardines (_Sardina pilchardus_) between the years 1990-2000?
Morocco caught the most sardines between 1990 to 2000.

```r
fisheries_tidy %>%
  select(country, asfis_species_name, year,catch)%>%
  filter(asfis_species_name=='Sardina pilchardus')%>%
  filter(between(year,1990,2000))%>%
  group_by(country)%>%
  summarise(most_sardines=sum(catch,na.rm=T))
```

```
## # A tibble: 37 × 2
##    country  most_sardines
##    <fct>            <dbl>
##  1 Albania            233
##  2 Algeria            333
##  3 Belize              15
##  4 Bulgaria            55
##  5 China                8
##  6 Croatia            220
##  7 Cyprus              17
##  8 Denmark            477
##  9 Estonia             43
## 10 France             966
## # … with 27 more rows
```

8. Which five countries caught the most cephalopods between 2008-2012?
India,China,Spain,Algeria,France.

```r
five_countries <- fisheries_tidy %>%
  select(country, asfis_species_name, year,catch)%>%
  filter(asfis_species_name=='Cephalopoda')%>%
  filter(between(year,2008,2012))%>%
  group_by(country)%>%
  summarise(most_caph=sum(catch,na.rm=T))
```


```r
five_countries%>%
  arrange(desc(most_caph))
```

```
## # A tibble: 16 × 2
##    country                  most_caph
##    <fct>                        <dbl>
##  1 India                          570
##  2 China                          257
##  3 Spain                          198
##  4 Algeria                        162
##  5 France                         101
##  6 Mauritania                      90
##  7 TimorLeste                      76
##  8 Italy                           66
##  9 Mozambique                      16
## 10 Cambodia                        15
## 11 Taiwan Province of China        13
## 12 Madagascar                      11
## 13 Croatia                          7
## 14 Israel                           0
## 15 Somalia                          0
## 16 Viet Nam                         0
```

9. Which species had the highest catch total between 2008-2012? (hint: Osteichthyes is not a species)
Osteichthyes had the highest catch total between 2008 to 2012.

```r
specieshighest <- fisheries_tidy %>%
  select(asfis_species_name, year,catch)%>%
  filter(between(year,2008,2012))%>%
  group_by(asfis_species_name)%>%
  summarise(highest_catch=sum(catch,na.rm=T))
```



```r
specieshighest%>%
  arrange(desc(highest_catch))
```

```
## # A tibble: 1,472 × 2
##    asfis_species_name    highest_catch
##    <chr>                         <dbl>
##  1 Osteichthyes                 107808
##  2 Theragra chalcogramma         41075
##  3 Engraulis ringens             35523
##  4 Katsuwonus pelamis            32153
##  5 Trichiurus lepturus           30400
##  6 Clupea harengus               28527
##  7 Thunnus albacares             20119
##  8 Scomber japonicus             14723
##  9 Gadus morhua                  13253
## 10 Thunnus alalunga              12019
## # … with 1,462 more rows
```

10. Use the data to do at least one analysis of your choice.
Which species had the highest catch total among all collected data years?
Osteichthyes had the highest catch total among all collected data years.

```r
highestcatch_all <- fisheries_tidy %>%
  select(asfis_species_name, year,catch)%>%
  group_by(asfis_species_name)%>%
  summarise(highesttotal=sum(catch,na.rm=T))
```

```r
highestcatch_all%>%
  arrange(desc(highesttotal))
```

```
## # A tibble: 1,546 × 2
##    asfis_species_name      highesttotal
##    <chr>                          <dbl>
##  1 Osteichthyes                  863949
##  2 Engraulis ringens             540739
##  3 Theragra chalcogramma         473722
##  4 Clupea harengus               264304
##  5 Gadus morhua                  217930
##  6 Katsuwonus pelamis            194923
##  7 Sardinops sagax               176094
##  8 Scomber japonicus             175444
##  9 Trachurus murphyi             152034
## 10 Sardinops melanostictus       144774
## # … with 1,536 more rows
```

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
