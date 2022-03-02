---
title: "Lab 13 Homework"
author: "Qianyu Jin"
date: "2022-03-02"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above. For any included plots, make sure they are clearly labeled. You are free to use any plot type that you feel best communicates the results of your analysis.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Libraries

```r
#install.packages("shinydashboard")
library(tidyverse)
library(shiny)
library(shinydashboard)
```

## Choose Your Adventure!
For this homework assignment, you have two choices of data. You only need to build an app for one of them. The first dataset is focused on UC Admissions and the second build on the Gabon data that we used for midterm 1.  

## Option 1
The data for this assignment come from the [University of California Information Center](https://www.universityofcalifornia.edu/infocenter). Admissions data were collected for the years 2010-2019 for each UC campus. Admissions are broken down into three categories: applications, admits, and enrollees. The number of individuals in each category are presented by demographic.  

**1. Load the `UC_admit.csv` data and use the function(s) of your choice to get an idea of the overall structure of the data frame, including its dimensions, column names, variable classes, etc. As part of this, determine if there are NA's and how they are treated.**  

```r
ucadmission <- readr::read_csv("data/uc_data/UC_admit.csv")
```

```
## Rows: 2160 Columns: 6
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (4): Campus, Category, Ethnicity, Perc FR
## dbl (2): Academic_Yr, FilteredCountFR
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
summary(ucadmission)
```

```
##     Campus           Academic_Yr     Category          Ethnicity        
##  Length:2160        Min.   :2010   Length:2160        Length:2160       
##  Class :character   1st Qu.:2012   Class :character   Class :character  
##  Mode  :character   Median :2014   Mode  :character   Mode  :character  
##                     Mean   :2014                                        
##                     3rd Qu.:2017                                        
##                     Max.   :2019                                        
##                                                                         
##    Perc FR          FilteredCountFR   
##  Length:2160        Min.   :     1.0  
##  Class :character   1st Qu.:   447.5  
##  Mode  :character   Median :  1837.0  
##                     Mean   :  7142.6  
##                     3rd Qu.:  6899.5  
##                     Max.   :113755.0  
##                     NA's   :1
```


**2. The president of UC has asked you to build a shiny app that shows admissions by ethnicity across all UC campuses. Your app should allow users to explore year, campus, and admit category as interactive variables. Use shiny dashboard and try to incorporate the aesthetics you have learned in ggplot to make the app neat and clean.**

```r
ucadmission <- 
  ucadmission %>%
  filter(!Ethnicity %in% c("All"))
ucadmission
```

```
## # A tibble: 1,890 × 6
##    Campus Academic_Yr Category   Ethnicity        `Perc FR` FilteredCountFR
##    <chr>        <dbl> <chr>      <chr>            <chr>               <dbl>
##  1 Davis         2019 Applicants International    21.16%              16522
##  2 Davis         2019 Applicants Unknown          2.51%                1959
##  3 Davis         2019 Applicants White            18.39%              14360
##  4 Davis         2019 Applicants Asian            30.76%              24024
##  5 Davis         2019 Applicants Chicano/Latino   22.44%              17526
##  6 Davis         2019 Applicants American Indian  0.35%                 277
##  7 Davis         2019 Applicants African American 4.39%                3425
##  8 Davis         2018 Applicants International    19.87%              15507
##  9 Davis         2018 Applicants Unknown          2.83%                2208
## 10 Davis         2018 Applicants White            18.96%              14797
## # … with 1,880 more rows
```


```r
library(shiny)

ui <- dashboardPage(
  dashboardHeader(title = "UC Admission by Ethnicity"),
  dashboardSidebar(disable = T),
  dashboardBody(
    fluidRow(
      box(title = "Plot Options", width = 3,
      selectInput("x", "Select Campus", choices = c("Davis", "Berkeley", "Irvine", "Los_Angeles", "Merced", "Riverside", "San_Diego", "Santa_Barbara", "Santa_Cruz"), selected = "Davis"),
      selectInput("y", "Select Year", choices = c("2010", "2011", "2012", "2013", "2014", "2015", "2016", "2017", "2018", "2019"), selected = "2019"),
      selectInput("z", "Select Category", choices = c("Applicants", "Admits", "Enrollees"), selected = "Enrollees")), 
    box(title = "UC Admission", width = 7,
    plotOutput("plot", width = "600px", height = "500px")
    ) 
    ) 
    ) 
)
 

server <- function(input, output, session) {
  output$plot <- renderPlot({
    ucadmission %>% 
      filter(Campus==input$x, Academic_Yr==input$y, Category==input$z) %>% 
      ggplot(aes(y=Ethnicity, x=FilteredCountFR)) +
      geom_col(color="black", fill= "cyan3", alpha=0.6) + 
      theme_light(base_size = 18) + 
      theme(axis.text.x = element_text(angle = 60, hjust = 1)) + 
      labs(x = "Number of Students",
          y = "Ethnicity")
  })
  session$onSessionEnded(stopApp)
  }


shinyApp(ui, server)
```

```
## PhantomJS not found. You can install it with webshot::install_phantomjs(). If it is installed, please make sure the phantomjs executable can be found via the PATH variable.
```

`<div style="width: 100% ; height: 400px ; text-align: center; box-sizing: border-box; -moz-box-sizing: border-box; -webkit-box-sizing: border-box;" class="muted well">Shiny applications not supported in static R Markdown documents</div>`{=html}


**3. Make alternate version of your app above by tracking enrollment at a campus over all of the represented years while allowing users to interact with campus, category, and ethnicity.**  

```r
library(shiny)
ui <- dashboardPage(
  dashboardHeader(title = "UC Admission by Year"),
  dashboardSidebar(disable = T),
  dashboardBody(
    fluidRow(
      box(title = "Plot Options", width = 3,
      selectInput("x", "Select Campus", choices = c("Davis", "Berkeley", "Irvine", "Los_Angeles", "Merced", "Riverside", "San_Diego", "Santa_Barbara", "Santa_Cruz"), selected = "Davis"),
      selectInput("y", "Select Ethnicity", choices = c("White", "Unknown", "International", "Chicano/Latino", "Asian", "American Indian", "Afeican American"), selected = "White"),
      selectInput("z", "Select Category", choices = c("Applicants", "Admits", "Enrollees"), selected = "Enrollees")), 
    box(title = "UC Admission", width = 7,
    plotOutput("plot", width = "600px", height = "500px")
    ) 
    ) 
    ) 
)
 

server <- function(input, output, session) {
  output$plot <- renderPlot({
    ucadmission %>% 
      filter(Campus==input$x, Ethnicity==input$y, Category==input$z) %>% 
      ggplot(aes(x=Academic_Yr, y=FilteredCountFR)) +
      geom_col(color="black", fill= "cyan3", alpha=0.6) + 
      theme_light(base_size = 18) + 
      theme(axis.text.x = element_text(angle = 60, hjust = 1)) + 
      labs(x = "Year",
          y = "Number of Students")
  })
  session$onSessionEnded(stopApp)
  }


shinyApp(ui, server)
```

`<div style="width: 100% ; height: 400px ; text-align: center; box-sizing: border-box; -moz-box-sizing: border-box; -webkit-box-sizing: border-box;" class="muted well">Shiny applications not supported in static R Markdown documents</div>`{=html}


## Option 2
We will use data from a study on vertebrate community composition and impacts from defaunation in Gabon, Africa. Reference: Koerner SE, Poulsen JR, Blanchard EJ, Okouyi J, Clark CJ. Vertebrate community composition and diversity declines along a defaunation gradient radiating from rural villages in Gabon. _Journal of Applied Ecology_. 2016.   

**1. Load the `IvindoData_DryadVersion.csv` data and use the function(s) of your choice to get an idea of the overall structure, including its dimensions, column names, variable classes, etc. As part of this, determine if NA's are present and how they are treated.**  

**2. Build an app that re-creates the plots shown on page 810 of this paper. The paper is included in the folder. It compares the relative abundance % to the distance from villages in rural Gabon. Use shiny dashboard and add aesthetics to the plot.  **  

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences. 
