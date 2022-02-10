---
title: "Lab 10 Homework"
author: "Please Add Your Name Here"
date: "2022-02-10"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above. For any included plots, make sure they are clearly labeled. You are free to use any plot type that you feel best communicates the results of your analysis.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the libraries

```r
library(tidyverse)
library(janitor)
library(here)
library(naniar)
```

## Desert Ecology
For this assignment, we are going to use a modified data set on [desert ecology](http://esapubs.org/archive/ecol/E090/118/). The data are from: S. K. Morgan Ernest, Thomas J. Valone, and James H. Brown. 2009. Long-term monitoring and experimental manipulation of a Chihuahuan Desert ecosystem near Portal, Arizona, USA. Ecology 90:1708.

```r
deserts <- read_csv(here("lab10", "data", "surveys_complete.csv"))
```

```
## Rows: 34786 Columns: 13
```

```
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (6): species_id, sex, genus, species, taxa, plot_type
## dbl (7): record_id, month, day, year, plot_id, hindfoot_length, weight
```

```
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

1. Use the function(s) of your choice to get an idea of its structure, including how NA's are treated. Are the data tidy?


```r
glimpse(deserts)
```

```
## Rows: 34,786
## Columns: 13
## $ record_id       <dbl> 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16,…
## $ month           <dbl> 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, …
## $ day             <dbl> 16, 16, 16, 16, 16, 16, 16, 16, 16, 16, 16, 16, 16, 16…
## $ year            <dbl> 1977, 1977, 1977, 1977, 1977, 1977, 1977, 1977, 1977, …
## $ plot_id         <dbl> 2, 3, 2, 7, 3, 1, 2, 1, 1, 6, 5, 7, 3, 8, 6, 4, 3, 2, …
## $ species_id      <chr> "NL", "NL", "DM", "DM", "DM", "PF", "PE", "DM", "DM", …
## $ sex             <chr> "M", "M", "F", "M", "M", "M", "F", "M", "F", "F", "F",…
## $ hindfoot_length <dbl> 32, 33, 37, 36, 35, 14, NA, 37, 34, 20, 53, 38, 35, NA…
## $ weight          <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ genus           <chr> "Neotoma", "Neotoma", "Dipodomys", "Dipodomys", "Dipod…
## $ species         <chr> "albigula", "albigula", "merriami", "merriami", "merri…
## $ taxa            <chr> "Rodent", "Rodent", "Rodent", "Rodent", "Rodent", "Rod…
## $ plot_type       <chr> "Control", "Long-term Krat Exclosure", "Control", "Rod…
```


```r
deserts
```

```
## # A tibble: 34,786 × 13
##    record_id month   day  year plot_id species_id sex   hindfoot_length weight
##        <dbl> <dbl> <dbl> <dbl>   <dbl> <chr>      <chr>           <dbl>  <dbl>
##  1         1     7    16  1977       2 NL         M                  32     NA
##  2         2     7    16  1977       3 NL         M                  33     NA
##  3         3     7    16  1977       2 DM         F                  37     NA
##  4         4     7    16  1977       7 DM         M                  36     NA
##  5         5     7    16  1977       3 DM         M                  35     NA
##  6         6     7    16  1977       1 PF         M                  14     NA
##  7         7     7    16  1977       2 PE         F                  NA     NA
##  8         8     7    16  1977       1 DM         M                  37     NA
##  9         9     7    16  1977       1 DM         F                  34     NA
## 10        10     7    16  1977       6 PF         F                  20     NA
## # … with 34,776 more rows, and 4 more variables: genus <chr>, species <chr>,
## #   taxa <chr>, plot_type <chr>
```

```r
naniar::miss_var_summary(deserts)
```

```
## # A tibble: 13 × 3
##    variable        n_miss pct_miss
##    <chr>            <int>    <dbl>
##  1 hindfoot_length   3348     9.62
##  2 weight            2503     7.20
##  3 sex               1748     5.03
##  4 record_id            0     0   
##  5 month                0     0   
##  6 day                  0     0   
##  7 year                 0     0   
##  8 plot_id              0     0   
##  9 species_id           0     0   
## 10 genus                0     0   
## 11 species              0     0   
## 12 taxa                 0     0   
## 13 plot_type            0     0
```


Data is tidy.

2. How many genera and species are represented in the data? What are the total number of observations? Which species is most/ least frequently sampled in the study?


```r
deserts %>%
  summarise(n_genera = n_distinct(genus),
            n_species = n_distinct(species),
            total = n())
```

```
## # A tibble: 1 × 3
##   n_genera n_species total
##      <int>     <int> <int>
## 1       26        40 34786
```


```r
deserts %>%
  count(species) %>%
  arrange(n)
```

```
## # A tibble: 40 × 2
##    species          n
##    <chr>        <int>
##  1 clarki           1
##  2 scutalatus       1
##  3 tereticaudus     1
##  4 tigris           1
##  5 uniparens        1
##  6 viridis          1
##  7 leucophrys       2
##  8 savannarum       2
##  9 fuscus           5
## 10 undulatus        5
## # … with 30 more rows
```

```r
deserts %>%
  count(species) %>%
  arrange(desc(n))
```

```
## # A tibble: 40 × 2
##    species          n
##    <chr>        <int>
##  1 merriami     10596
##  2 penicillatus  3123
##  3 ordii         3027
##  4 baileyi       2891
##  5 megalotis     2609
##  6 spectabilis   2504
##  7 torridus      2249
##  8 flavus        1597
##  9 eremicus      1299
## 10 albigula      1252
## # … with 30 more rows
```


3. What is the proportion of taxa included in this study? Show a table and plot that reflects this count.


```r
deserts %>%
  count(taxa)
```

```
## # A tibble: 4 × 2
##   taxa        n
##   <chr>   <int>
## 1 Bird      450
## 2 Rabbit     75
## 3 Reptile    14
## 4 Rodent  34247
```


```r
deserts %>%
  ggplot(aes(x=taxa, fill=taxa)) + geom_bar() + scale_y_log10()
```

![](lab10_hw_files/figure-html/unnamed-chunk-10-1.png)<!-- -->


4. For the taxa included in the study, use the fill option to show the proportion of individuals sampled by `plot_type.`


```r
deserts %>%
  ggplot(aes(x=taxa, fill=plot_type)) + geom_bar(position = "dodge") + scale_y_log10() + coord_flip()
```

![](lab10_hw_files/figure-html/unnamed-chunk-11-1.png)<!-- -->


5. What is the range of weight for each species included in the study? Remove any observations of weight that are NA so they do not show up in the plot.


```r
deserts %>%
  filter(weight != "NA") %>%
  ggplot(aes(x=species, y=weight)) + geom_boxplot() + theme(axis.text.x = element_text(angle = 60, hjust = 1))
```

![](lab10_hw_files/figure-html/unnamed-chunk-12-1.png)<!-- -->


6. Add another layer to your answer from #4 using `geom_point` to get an idea of how many measurements were taken for each species.


```r
deserts %>%
  filter(weight != "NA") %>%
  ggplot(aes(x=species, y=weight)) + geom_boxplot() + theme(axis.text.x = element_text(angle = 60, hjust = 1)) + geom_point(alpha = .3) + coord_flip()
```

![](lab10_hw_files/figure-html/unnamed-chunk-13-1.png)<!-- -->
Didn't make sense to use it with part 4 since we already had a count for each of the species... Maybe part 5?

7. [Dipodomys merriami](https://en.wikipedia.org/wiki/Merriam's_kangaroo_rat) is the most frequently sampled animal in the study. How have the number of observations of this species changed over the years included in the study?


```r
deserts %>%
  filter(species == "merriami" & genus == "Dipodomys") %>%
  group_by(year) %>%
  summarise(n_sample = n())
```

```
## # A tibble: 26 × 2
##     year n_sample
##    <dbl>    <int>
##  1  1977      264
##  2  1978      389
##  3  1979      209
##  4  1980      493
##  5  1981      559
##  6  1982      609
##  7  1983      528
##  8  1984      396
##  9  1985      667
## 10  1986      406
## # … with 16 more rows
```


8. What is the relationship between `weight` and `hindfoot` length? Consider whether or not over plotting is an issue.


```r
deserts %>%
  ggplot(aes(x=weight,y=hindfoot_length)) +geom_point(color = "dark red", alpa = .2) + geom_smooth(method=lm, se=F) 
```

```
## Warning: Ignoring unknown parameters: alpa
```

```
## `geom_smooth()` using formula 'y ~ x'
```

```
## Warning: Removed 4048 rows containing non-finite values (stat_smooth).
```

```
## Warning: Removed 4048 rows containing missing values (geom_point).
```

![](lab10_hw_files/figure-html/unnamed-chunk-15-1.png)<!-- -->


9. Which two species have, on average, the highest weight? Once you have identified them, make a new column that is a ratio of `weight` to `hindfoot_length`. Make a plot that shows the range of this new ratio and fill by sex.


```r
deserts %>%
  filter(weight!="NA") %>%
  group_by(species) %>%
  summarise(mean_weight = mean(weight)) %>%
  arrange(desc(mean_weight))
```

```
## # A tibble: 22 × 2
##    species      mean_weight
##    <chr>              <dbl>
##  1 albigula           159. 
##  2 spectabilis        120. 
##  3 spilosoma           93.5
##  4 hispidus            65.6
##  5 fulviventer         58.9
##  6 ochrognathus        55.4
##  7 ordii               48.9
##  8 merriami            43.2
##  9 baileyi             31.7
## 10 leucogaster         31.6
## # … with 12 more rows
```


```r
desert_ratio <- deserts %>%
  filter(species == "albigula" | species == "spectabilis") %>%
  filter(weight!="NA" & hindfoot_length != "NA" & sex != "NA") %>%
  group_by(species) %>%
  mutate(ratiowl = weight/hindfoot_length) %>%
  select(species, sex, weight, hindfoot_length, ratiowl)
desert_ratio
```

```
## # A tibble: 3,068 × 5
## # Groups:   species [2]
##    species     sex   weight hindfoot_length ratiowl
##    <chr>       <chr>  <dbl>           <dbl>   <dbl>
##  1 spectabilis F        117              50    2.34
##  2 spectabilis F        121              51    2.37
##  3 spectabilis M        115              51    2.25
##  4 spectabilis F        120              48    2.5 
##  5 spectabilis F        118              48    2.46
##  6 spectabilis F        126              52    2.42
##  7 spectabilis M        132              50    2.64
##  8 spectabilis F        122              53    2.30
##  9 spectabilis F        107              48    2.23
## 10 spectabilis F        115              50    2.3 
## # … with 3,058 more rows
```


```r
desert_ratio %>%
  ggplot(aes(x=species, y =ratiowl, fill=sex)) + geom_boxplot() 
```

![](lab10_hw_files/figure-html/unnamed-chunk-18-1.png)<!-- -->


10. Make one plot of your choice! Make sure to include at least two of the aesthetics options you have learned.


```r
desert_choice <- deserts %>%
  group_by(year) %>%
  mutate(n_sample = n()) %>%
  select(species, sex, weight, hindfoot_length, n_sample,year)
desert_choice
```

```
## # A tibble: 34,786 × 6
## # Groups:   year [26]
##    species  sex   weight hindfoot_length n_sample  year
##    <chr>    <chr>  <dbl>           <dbl>    <int> <dbl>
##  1 albigula M         NA              32      487  1977
##  2 albigula M         NA              33      487  1977
##  3 merriami F         NA              37      487  1977
##  4 merriami M         NA              36      487  1977
##  5 merriami M         NA              35      487  1977
##  6 flavus   M         NA              14      487  1977
##  7 eremicus F         NA              NA      487  1977
##  8 merriami M         NA              37      487  1977
##  9 merriami F         NA              34      487  1977
## 10 flavus   F         NA              20      487  1977
## # … with 34,776 more rows
```

```r
desert_choice %>%
  filter(sex != "NA") %>%
  ggplot(aes(x=year, y = n_sample, fill = sex)) +geom_col(position = "dodge") + scale_y_log10() + coord_flip() + labs(title = "Sample Size by Sex over Time",
       x = "Year",
       y = "Logmarthmic Count",
       fill = "Sex")
```

![](lab10_hw_files/figure-html/unnamed-chunk-20-1.png)<!-- -->


## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences. 
