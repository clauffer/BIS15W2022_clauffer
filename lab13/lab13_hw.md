---
title: "Lab 13 Homework"
author: "Please Add Your Name Here"
date: "2022-03-01"
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
library(tidyverse)
library(shiny)
library(janitor)
library(shinydashboard)
library(naniar)
```

## Choose Your Adventure!
For this homework assignment, you have two choices of data. You only need to build an app for one of them. The first dataset is focused on UC Admissions and the second build on the Gabon data that we used for midterm 1.  

## Option 1
The data for this assignment come from the [University of California Information Center](https://www.universityofcalifornia.edu/infocenter). Admissions data were collected for the years 2010-2019 for each UC campus. Admissions are broken down into three categories: applications, admits, and enrollees. The number of individuals in each category are presented by demographic.  

**1. Load the `UC_admit.csv` data and use the function(s) of your choice to get an idea of the overall structure of the data frame, including its dimensions, column names, variable classes, etc. As part of this, determine if there are NA's and how they are treated.**  


```r
ucadmit <- readr::read_csv("data/uc_data/UC_admit.csv")
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
naniar::miss_var_summary(ucadmit)
```

```
## # A tibble: 6 × 3
##   variable        n_miss pct_miss
##   <chr>            <int>    <dbl>
## 1 Perc FR              1   0.0463
## 2 FilteredCountFR      1   0.0463
## 3 Campus               0   0     
## 4 Academic_Yr          0   0     
## 5 Category             0   0     
## 6 Ethnicity            0   0
```


```r
glimpse(ucadmit)
```

```
## Rows: 2,160
## Columns: 6
## $ Campus          <chr> "Davis", "Davis", "Davis", "Davis", "Davis", "Davis", …
## $ Academic_Yr     <dbl> 2019, 2019, 2019, 2019, 2019, 2019, 2019, 2019, 2018, …
## $ Category        <chr> "Applicants", "Applicants", "Applicants", "Applicants"…
## $ Ethnicity       <chr> "International", "Unknown", "White", "Asian", "Chicano…
## $ `Perc FR`       <chr> "21.16%", "2.51%", "18.39%", "30.76%", "22.44%", "0.35…
## $ FilteredCountFR <dbl> 16522, 1959, 14360, 24024, 17526, 277, 3425, 78093, 15…
```



```r
ucadmit_clean <- ucadmit %>%
  filter(`Perc FR` != "NA") %>%
  filter(FilteredCountFR != "NA") %>%
  clean_names()
```


```r
naniar::miss_var_summary(ucadmit_clean)
```

```
## # A tibble: 6 × 3
##   variable          n_miss pct_miss
##   <chr>              <int>    <dbl>
## 1 campus                 0        0
## 2 academic_yr            0        0
## 3 category               0        0
## 4 ethnicity              0        0
## 5 perc_fr                0        0
## 6 filtered_count_fr      0        0
```


**2. The president of UC has asked you to build a shiny app that shows admissions by ethnicity across all UC campuses. Your app should allow users to explore year, campus, and admit category as interactive variables. Use shiny dashboard and try to incorporate the aesthetics you have learned in ggplot to make the app neat and clean.**


```r
ui <- 
  dashboardPage(
  dashboardHeader(title = "UC Admission"),
  dashboardSidebar(disable =  T),
  dashboardBody(
  fluidRow(
  box(title = "Plot Options", width = 3,


  selectInput("x", "Select Campus", choices = c("Davis", "Berkeley", "Santa_Barbara", "Irvine", "Merced", "Los_Angeles", "Santa_Cruz", "San_Diego", "Riverside"),
              selected = "Davis"),
    selectInput("y", "Select Year", choices = c("2010", "2011", "2013", "2014","2015", "2016", "2017", "2018", "2019"),
              selected = "2010"),
    checkboxGroupInput("z", "Select Category", choices = c("Admits", "Applicants", "Enrollees"),
              selected = "Admits"),
    ),
    box(title = "Plot of UC Admission Data by Ethnicity", width = 9,
        plotOutput("plot", width = "600px", height = "500px"))
    
    )
  
  
    )
)

server <- function(input, output,session) {
  
  session$onSessionEnded(stopApp)
  
  output$plot <- renderPlot({
    
    ucadmit_clean %>%
    filter(campus == input$x) %>%
    filter(academic_yr == input$y) %>%
    filter(category == input$z) %>%
    ggplot(aes_string(x = "ethnicity", y = "filtered_count_fr")) + 
    geom_col() + 
    labs(x = "Ethnicity", y= "Admission Count") +
    coord_flip() + 
    theme_classic(base_size = 18)
  })
  
}

shinyApp(ui, server)
```

`<div style="width: 100% ; height: 400px ; text-align: center; box-sizing: border-box; -moz-box-sizing: border-box; -webkit-box-sizing: border-box;" class="muted well">Shiny applications not supported in static R Markdown documents</div>`{=html}



**3. Make alternate version of your app above by tracking enrollment at a campus over all of the represented years while allowing users to interact with campus, category, and ethnicity.**  


```r
head(ucadmit$Ethnicity)
```

```
## [1] "International"   "Unknown"         "White"           "Asian"          
## [5] "Chicano/Latino"  "American Indian"
```



```r
ui <- 
  dashboardPage(
  dashboardHeader(title = "UC Admission"),
  dashboardSidebar(disable =  T),
  dashboardBody(
  fluidRow(
  box(title = "Plot Options", width = 3,


  selectInput("x", "Select Campus", choices = c("Davis", "Berkeley", "Santa_Barbara", "Irvine", "Merced", "Los_Angeles", "Santa_Cruz", "San_Diego", "Riverside"),
              selected = "Davis"),
    selectInput("y", "Select Ethnicity", choices = c("American Indian", "Chicano/Latino", "Asian", "International","Unknown", "White"),
                selected = "American Indian"),
    checkboxGroupInput("z", "Select Category", choices = c("Admits", "Applicants", "Enrollees"),
              selected = "Admits"),
    ),
    box(title = "Plot of UC Admission Data by Year", width = 9,
        plotOutput("plot", width = "600px", height = "500px"))
    
    )
  
  
    )
)

server <- function(input, output,session) {
  
  session$onSessionEnded(stopApp)
  
  output$plot <- renderPlot({
    
    ucadmit_clean %>%
    filter(campus == input$x) %>%
    filter(ethnicity == input$y) %>%
    filter(category == input$z) %>%
    ggplot(aes_string(x = "academic_yr", y = "filtered_count_fr")) + 
    geom_col() + 
    labs(x = "Academic Year", y= "Admission Count") +
    theme_classic(base_size = 18)
  })
  
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
