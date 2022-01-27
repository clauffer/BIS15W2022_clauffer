---
title: "Midterm 1"
author: "Colin Lauffer"
date: "2022-01-27"
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
library("tidyverse")
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
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```r
library("janitor")
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

```r
library("skimr")
```

## Questions  
Wikipedia's definition of [data science](https://en.wikipedia.org/wiki/Data_science): "Data science is an interdisciplinary field that uses scientific methods, processes, algorithms and systems to extract knowledge and insights from noisy, structured and unstructured data, and apply knowledge and actionable insights from data across a broad range of application domains."  

1. (2 points) Consider the definition of data science above. Although we are only part-way through the quarter, what specific elements of data science do you feel we have practiced? Provide at least one specific example.  

I feel we have practiced process and systems to extracts knowledge and insights from unstructured and structured data. I feel like because for example we have learned to remove NA values (unstructured) and also worked on process to extract data such as SuperHeros name when given that they are short and heavy.

2. (2 points) What is the most helpful or interesting thing you have learned so far in BIS 15L? What is something that you think needs more work or practice?  

The most helpful thing I feel has been learning to actually use a working directory because it has helped me outside of class along with actually accessing data using readr commands.

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

```r
skim(elephants)
```


Table: Data summary

|                         |          |
|:------------------------|:---------|
|Name                     |elephants |
|Number of rows           |288       |
|Number of columns        |3         |
|_______________________  |          |
|Column type frequency:   |          |
|character                |1         |
|numeric                  |2         |
|________________________ |          |
|Group variables          |None      |


**Variable type: character**

|skim_variable | n_missing| complete_rate| min| max| empty| n_unique| whitespace|
|:-------------|---------:|-------------:|---:|---:|-----:|--------:|----------:|
|Sex           |         0|             1|   1|   1|     0|        2|          0|


**Variable type: numeric**

|skim_variable | n_missing| complete_rate|   mean|   sd|    p0|    p25|    p50|    p75|   p100|hist  |
|:-------------|---------:|-------------:|------:|----:|-----:|------:|------:|------:|------:|:-----|
|Age           |         0|             1|  10.97|  8.4|  0.01|   4.58|   9.46|  16.50|  32.17|▆▇▂▂▂ |
|Height        |         0|             1| 187.68| 50.6| 75.46| 160.75| 200.00| 221.09| 304.06|▃▃▇▇▁ |

```r
head(elephants)
```

```
## # A tibble: 6 × 3
##     Age Height Sex  
##   <dbl>  <dbl> <chr>
## 1   1.4    120 M    
## 2  17.5    227 M    
## 3  12.8    235 M    
## 4  11.2    210 M    
## 5  12.7    220 M    
## 6  12.7    189 M
```


4. (2 points) Change the names of the variables to lower case and change the class of the variable `sex` to a factor.



```r
elephants_clean <- clean_names(elephants)
```


```r
elephants_clean$sex <- as.factor(elephants_clean$sex)
class(elephants_clean$sex)
```

```
## [1] "factor"
```


5. (2 points) How many male and female elephants are represented in the data?


```r
tabyl(elephants_clean$sex)
```

```
##  elephants_clean$sex   n   percent
##                    F 150 0.5208333
##                    M 138 0.4791667
```


6. (2 points) What is the average age all elephants in the data?


```r
mean(elephants_clean$age)
```

```
## [1] 10.97132
```


7. (2 points) How does the average age and height of elephants compare by sex?



```r
elephants_male <- data.frame(elephants_clean %>% 
filter(sex == "M"))


elephants_male %>%
  summarise(mean_age = mean(age),
          mean_height = mean(height))
```

```
##   mean_age mean_height
## 1 8.945145    185.1312
```

```r
elephants_female <- data.frame(elephants_clean %>% 
filter(sex == "F"))
elephants_female %>%
  summarise(mean_age = mean(age),
          mean_height = mean(height))
```

```
##   mean_age mean_height
## 1  12.8354    190.0307
```
Female events tend to live longer and grow higher.

8. (2 points) How does the average height of elephants compare by sex for individuals over 20 years old. Include the min and max height as well as the number of individuals in the sample as part of your analysis.  


```r
elephants_male %>%
  filter(age >20) %>%
  summarise(mean_height = mean(height),
            max_height = max(height),
            min_height = min(height),
            total = n())
```

```
##   mean_height max_height min_height total
## 1    269.5931     304.06     228.69    13
```

```r
elephants_female %>%
  filter(age >20) %>%
  summarise(mean_height = mean(height),
            max_height = max(height),
            min_height = min(height),
            total = n())
```

```
##   mean_height max_height min_height total
## 1    232.2014      277.8     192.54    37
```
Male events have a higher height but there are a lot lessmales than females.

For the next series of questions, we will use data from a study on vertebrate community composition and impacts from defaunation in [Gabon, Africa](https://en.wikipedia.org/wiki/Gabon). One thing to notice is that the data include 24 separate transects. Each transect represents a path through different forest management areas.  

Reference: Koerner SE, Poulsen JR, Blanchard EJ, Okouyi J, Clark CJ. Vertebrate community composition and diversity declines along a defaunation gradient radiating from rural villages in Gabon. _Journal of Applied Ecology_. 2016. This paper, along with a description of the variables is included inside the midterm 1 folder.  

9. (2 points) Load `IvindoData_DryadVersion.csv` and use the function(s) of your choice to get an idea of the overall structure. Change the variables `HuntCat` and `LandUse` to factors.


```r
gabon <- readr::read_csv("data/IvindoData_DryadVersion.csv")
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
skim(gabon)
```


Table: Data summary

|                         |      |
|:------------------------|:-----|
|Name                     |gabon |
|Number of rows           |24    |
|Number of columns        |26    |
|_______________________  |      |
|Column type frequency:   |      |
|character                |2     |
|numeric                  |24    |
|________________________ |      |
|Group variables          |None  |


**Variable type: character**

|skim_variable | n_missing| complete_rate| min| max| empty| n_unique| whitespace|
|:-------------|---------:|-------------:|---:|---:|-----:|--------:|----------:|
|HuntCat       |         0|             1|   4|   8|     0|        3|          0|
|LandUse       |         0|             1|   4|   7|     0|        3|          0|


**Variable type: numeric**

|skim_variable           | n_missing| complete_rate|  mean|    sd|    p0|   p25|   p50|   p75|  p100|hist  |
|:-----------------------|---------:|-------------:|-----:|-----:|-----:|-----:|-----:|-----:|-----:|:-----|
|TransectID              |         0|             1| 13.50|  8.51|  1.00|  5.75| 14.50| 20.25| 27.00|▇▃▅▆▆ |
|Distance                |         0|             1| 11.88|  7.28|  2.70|  5.67|  9.72| 17.68| 26.76|▇▂▂▅▂ |
|NumHouseholds           |         0|             1| 37.88| 17.80| 13.00| 24.75| 29.00| 54.00| 73.00|▇▇▂▇▂ |
|Veg_Rich                |         0|             1| 14.83|  2.07| 10.88| 13.10| 14.94| 16.54| 18.75|▃▂▃▇▁ |
|Veg_Stems               |         0|             1| 32.80|  5.96| 23.44| 28.69| 32.44| 37.08| 47.56|▆▇▆▃▁ |
|Veg_liana               |         0|             1| 11.04|  3.29|  4.75|  9.03| 11.94| 13.25| 16.38|▃▂▃▇▃ |
|Veg_DBH                 |         0|             1| 46.09| 10.67| 28.45| 40.65| 43.90| 50.57| 76.48|▂▇▃▁▁ |
|Veg_Canopy              |         0|             1|  3.47|  0.35|  2.50|  3.25|  3.43|  3.75|  4.00|▁▁▇▅▇ |
|Veg_Understory          |         0|             1|  3.02|  0.34|  2.38|  2.88|  3.00|  3.17|  3.88|▂▆▇▂▁ |
|RA_Apes                 |         0|             1|  2.04|  3.03|  0.00|  0.00|  0.48|  3.82| 12.93|▇▂▁▁▁ |
|RA_Birds                |         0|             1| 58.64| 14.71| 31.56| 52.51| 57.89| 68.18| 85.03|▅▅▇▇▃ |
|RA_Elephant             |         0|             1|  0.54|  0.67|  0.00|  0.00|  0.36|  0.89|  2.30|▇▂▂▁▁ |
|RA_Monkeys              |         0|             1| 31.30| 12.38|  5.84| 22.70| 31.74| 39.88| 54.12|▂▅▃▇▂ |
|RA_Rodent               |         0|             1|  3.28|  1.47|  1.06|  2.05|  3.23|  4.09|  6.31|▇▅▇▃▃ |
|RA_Ungulate             |         0|             1|  4.17|  4.31|  0.00|  1.23|  2.54|  5.16| 13.86|▇▂▁▁▂ |
|Rich_AllSpecies         |         0|             1| 20.21|  2.06| 15.00| 19.00| 20.00| 22.00| 24.00|▁▁▇▅▁ |
|Evenness_AllSpecies     |         0|             1|  0.77|  0.05|  0.67|  0.75|  0.78|  0.81|  0.83|▃▁▅▇▇ |
|Diversity_AllSpecies    |         0|             1|  2.31|  0.15|  1.97|  2.25|  2.32|  2.43|  2.57|▂▃▇▆▅ |
|Rich_BirdSpecies        |         0|             1| 10.33|  1.24|  8.00| 10.00| 11.00| 11.00| 13.00|▃▅▇▁▁ |
|Evenness_BirdSpecies    |         0|             1|  0.71|  0.08|  0.56|  0.68|  0.72|  0.77|  0.82|▅▁▇▆▇ |
|Diversity_BirdSpecies   |         0|             1|  1.66|  0.20|  1.16|  1.60|  1.68|  1.78|  2.01|▂▂▅▇▃ |
|Rich_MammalSpecies      |         0|             1|  9.88|  1.68|  6.00|  9.00| 10.00| 11.00| 12.00|▂▂▃▅▇ |
|Evenness_MammalSpecies  |         0|             1|  0.75|  0.06|  0.62|  0.71|  0.74|  0.78|  0.86|▂▃▇▂▅ |
|Diversity_MammalSpecies |         0|             1|  1.70|  0.17|  1.38|  1.57|  1.70|  1.81|  2.06|▅▇▇▇▃ |

```r
gabon$HuntCat <- as.factor(gabon$HuntCat)
gabon$LandUse <- as.factor(gabon$LandUse)
class(gabon$LandUse)
```

```
## [1] "factor"
```


10. (4 points) For the transects with high and moderate hunting intensity, how does the average diversity of birds and mammals compare?


```r
gabon %>%
  filter(HuntCat == "Moderate" | HuntCat == "High") %>%
  summarise(mean_birddiversity = mean(Diversity_BirdSpecies),
            mean_mamdiversity = mean(Diversity_MammalSpecies))
```

```
## # A tibble: 1 × 2
##   mean_birddiversity mean_mamdiversity
##                <dbl>             <dbl>
## 1               1.64              1.71
```
There is greater mammal diversity.

11. (4 points) One of the conclusions in the study is that the relative abundance of animals drops off the closer you get to a village. Let's try to reconstruct this (without the statistics). How does the relative abundance (RA) of apes, birds, elephants, monkeys, rodents, and ungulates compare between sites that are less than 3km from a village to sites that are greater than 25km from a village? The variable `Distance` measures the distance of the transect from the nearest village. Hint: try using the `across` operator.  


```r
gabon %>%
  filter(Distance < 3) %>%
  summarise(across(contains("RA"), mean))
```

```
## # A tibble: 1 × 7
##   TransectID RA_Apes RA_Birds RA_Elephant RA_Monkeys RA_Rodent RA_Ungulate
##        <dbl>   <dbl>    <dbl>       <dbl>      <dbl>     <dbl>       <dbl>
## 1         21    0.12     76.6       0.145       17.3      3.90        1.87
```

```r
gabon %>%
  filter(Distance > 25) %>%
  summarise(across(contains("RA"), mean))
```

```
## # A tibble: 1 × 7
##   TransectID RA_Apes RA_Birds RA_Elephant RA_Monkeys RA_Rodent RA_Ungulate
##        <dbl>   <dbl>    <dbl>       <dbl>      <dbl>     <dbl>       <dbl>
## 1         24    4.91     31.6           0       54.1      1.29        8.12
```
There tends to be a lot more biodviersity when it comes to monkeys and apes further away compared to rodents, birds,and elephants which have a higher population closer to towns.

12. (4 points) Based on your interest, do one exploratory analysis on the `gabon` data of your choice. This analysis needs to include a minimum of two functions in `dplyr.`


```r
gabon %>%
  select(LandUse, HuntCat, Distance, RA_Elephant) %>%
  filter(LandUse == "Logging") %>%
  summarise(mean_distance = mean(Distance),
            mean_RAEleph = mean(RA_Elephant),
            total = n())
```

```
## # A tibble: 1 × 3
##   mean_distance mean_RAEleph total
##           <dbl>        <dbl> <int>
## 1          10.8        0.425    13
```

```r
gabon %>%
  select(LandUse, HuntCat, Distance, RA_Elephant) %>%
  filter(LandUse != "Logging") %>%
  summarise(mean_distance = mean(Distance),
            mean_RAEleph = mean(RA_Elephant),
            total = n())
```

```
## # A tibble: 1 × 3
##   mean_distance mean_RAEleph total
##           <dbl>        <dbl> <int>
## 1          13.1        0.687    11
```
I checked to see how far logging was and the total impact on the Relative abudance of elephants. What I found was when Landuse was logging it was closer to town and there was less RA of elephants. But when landuse wasn't logging it was further away and there were more elephants.
