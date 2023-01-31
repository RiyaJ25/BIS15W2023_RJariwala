---
title: "Midterm 1"
author: "Please Add Your Name Here"
date: "2023-01-31"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



```r
getwd()
```

```
## [1] "C:/Users/riyaj/OneDrive/Desktop/BIS15W2023_RJariwala-main/practice_midterm1"
```

## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your code should be organized, clean, and run free from errors. Remember, you must remove the `#` for any included code chunks to run. Be sure to add your name to the author header above. You may use any resources to answer these questions (including each other), but you may not post questions to Open Stacks or external help sites. There are 15 total questions, each is worth 2 points.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the tidyverse
If you plan to use any other libraries to complete this assignment then you should load them here.

```r
library(tidyverse)
```

```
## Warning: package 'tidyverse' was built under R version 4.2.2
```

```
## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.2 ──
## ✔ ggplot2 3.4.0      ✔ purrr   1.0.0 
## ✔ tibble  3.1.8      ✔ dplyr   1.0.10
## ✔ tidyr   1.2.1      ✔ stringr 1.4.1 
## ✔ readr   2.1.3      ✔ forcats 0.5.2
```

```
## Warning: package 'ggplot2' was built under R version 4.2.2
```

```
## Warning: package 'tibble' was built under R version 4.2.2
```

```
## Warning: package 'tidyr' was built under R version 4.2.2
```

```
## Warning: package 'readr' was built under R version 4.2.2
```

```
## Warning: package 'purrr' was built under R version 4.2.2
```

```
## Warning: package 'dplyr' was built under R version 4.2.2
```

```
## Warning: package 'forcats' was built under R version 4.2.2
```

```
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
```

```r
library(janitor)
```

```
## Warning: package 'janitor' was built under R version 4.2.2
```

```
## 
## Attaching package: 'janitor'
## 
## The following objects are masked from 'package:stats':
## 
##     chisq.test, fisher.test
```

## Questions  

Wikipedia's definition of [data science](https://en.wikipedia.org/wiki/Data_science): "Data science is an interdisciplinary field that uses scientific methods, processes, algorithms and systems to extract knowledge and insights from noisy, structured and unstructured data, and apply knowledge and actionable insights from data across a broad range of application domains."  

1. (2 points) Consider the definition of data science above. Although we are only part-way through the quarter, what specific elements of data science do you feel we have practiced? Provide at least one specific example.  

2. (2 points) What is the most helpful or interesting thing you have learned so far in BIS 15L? What is something that you think needs more work or practice?  

In the midterm 1 folder there is a second folder called `data`. Inside the `data` folder, there is a .csv file called `ElephantsMF`. These data are from Phyllis Lee, Stirling University, and are related to Lee, P., et al. (2013), "Enduring consequences of early experiences: 40-year effects on survival and success among African elephants (Loxodonta africana)," Biology Letters, 9: 20130011. [kaggle](https://www.kaggle.com/mostafaelseidy/elephantsmf).  

3. (2 points) Please load these data as a new object called `elephants`. Use the function(s) of your choice to get an idea of the structure of the data. Be sure to show the class of each variable.

```r
elephants <- readr::read_csv("ElephantsMF.csv")
```

```
## Rows: 288 Columns: 3
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (1): Sex
## dbl (2): Age, Height
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```


```r
view(elephants)
```

4. (2 points) Change the names of the variables to lower case and change the class of the variable `sex` to a factor.

```r
elephants <- janitor::clean_names(elephants)
names(elephants)
```

```
## [1] "age"    "height" "sex"
```

```r
elephants$sex <- as.factor(elephants$sex)
class(elephants$sex)
```

```
## [1] "factor"
```


5. (2 points) How many male and female elephants are represented in the data?

```r
tabyl(elephants, sex)
```

```
##  sex   n   percent
##    F 150 0.5208333
##    M 138 0.4791667
```
#There are 150 females and 138 males. 

6. (2 points) What is the average age all elephants in the data?


```r
class(elephants$age)
```

```
## [1] "numeric"
```

```r
mean(elephants$age)
```

```
## [1] 10.97132
```
The average age is 10.97312 years. 

7. (2 points) How does the average age and height of elephants compare by sex?

```r
elephants %>% 
  filter(sex=="M") %>% 
  summarize(average_age=mean(age),
            average_height=mean(height)) 
```

```
## # A tibble: 1 × 2
##   average_age average_height
##         <dbl>          <dbl>
## 1        8.95           185.
```



```r
elephants %>% 
  filter(sex=="F") %>% 
  summarize(average_age=mean(age),
            average_height=mean(height)) 
```

```
## # A tibble: 1 × 2
##   average_age average_height
##         <dbl>          <dbl>
## 1        12.8           190.
```


8. (2 points) How does the average height of elephants compare by sex for individuals over 20 years old. Include the min and max height as well as the number of individuals in the sample as part of your analysis.  

```r
elephants %>% 
  filter(sex=="M", age>20) %>% 
  summarize(average_age=mean(age),
            average_height=mean(height),
            total = n())
```

```
## # A tibble: 1 × 3
##   average_age average_height total
##         <dbl>          <dbl> <int>
## 1        25.2           270.    13
```

```r
elephants %>% 
  filter(sex=="F", age>20) %>% 
  summarize(average_age=mean(age),
            average_height=mean(height),
            total = n())
```

```
## # A tibble: 1 × 3
##   average_age average_height total
##         <dbl>          <dbl> <int>
## 1        25.6           232.    37
```

For the next series of questions, we will use data from a study on vertebrate community composition and impacts from defaunation in [Gabon, Africa](https://en.wikipedia.org/wiki/Gabon). One thing to notice is that the data include 24 separate transects. Each transect represents a path through different forest management areas.  

Reference: Koerner SE, Poulsen JR, Blanchard EJ, Okouyi J, Clark CJ. Vertebrate community composition and diversity declines along a defaunation gradient radiating from rural villages in Gabon. _Journal of Applied Ecology_. 2016. This paper, along with a description of the variables is included inside the midterm 1 folder.  

9. (2 points) Load `IvindoData_DryadVersion.csv` and use the function(s) of your choice to get an idea of the overall structure. Change the variables `HuntCat` and `LandUse` to factors.

```r
vertebrates <- readr::read_csv("IvindoData_DryadVersion.csv")
```

```
## Rows: 24 Columns: 26
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr  (2): HuntCat, LandUse
## dbl (24): TransectID, Distance, NumHouseholds, Veg_Rich, Veg_Stems, Veg_lian...
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
class(vertebrates)
```

```
## [1] "spec_tbl_df" "tbl_df"      "tbl"         "data.frame"
```

```r
vertebrates <- janitor::clean_names(vertebrates)
vertebrates %>% mutate_all(tolower)
```

```
## # A tibble: 24 × 26
##    transect_id distance hunt_cat num_h…¹ land_…² veg_r…³ veg_s…⁴ veg_l…⁵ veg_dbh
##    <chr>       <chr>    <chr>    <chr>   <chr>   <chr>   <chr>   <chr>   <chr>  
##  1 1           7.14     moderate 54      park    16.67   31.2    5.78    49.57  
##  2 2           17.31    none     54      park    15.75   37.44   13.25   34.59  
##  3 2           18.32    none     29      park    16.88   32.33   4.75    42.82  
##  4 3           20.85    none     29      logging 12.44   29.39   9.78    36.62  
##  5 4           15.95    none     29      park    17.13   36      13.25   41.52  
##  6 5           17.47    none     29      park    16.5    29.22   12.88   44.07  
##  7 6           24.06    none     29      park    14.75   31.22   8.38    51.22  
##  8 7           19.81    none     54      logging 13.25   32.56   8.38    41.94  
##  9 8           5.78     high     25      neither 12.63   23.67   5.13    45.21  
## 10 9           5.13     high     73      logging 16      27.11   9.75    69.3   
## # … with 14 more rows, 17 more variables: veg_canopy <chr>,
## #   veg_understory <chr>, ra_apes <chr>, ra_birds <chr>, ra_elephant <chr>,
## #   ra_monkeys <chr>, ra_rodent <chr>, ra_ungulate <chr>,
## #   rich_all_species <chr>, evenness_all_species <chr>,
## #   diversity_all_species <chr>, rich_bird_species <chr>,
## #   evenness_bird_species <chr>, diversity_bird_species <chr>,
## #   rich_mammal_species <chr>, evenness_mammal_species <chr>, …
```

```r
names(vertebrates)
```

```
##  [1] "transect_id"              "distance"                
##  [3] "hunt_cat"                 "num_households"          
##  [5] "land_use"                 "veg_rich"                
##  [7] "veg_stems"                "veg_liana"               
##  [9] "veg_dbh"                  "veg_canopy"              
## [11] "veg_understory"           "ra_apes"                 
## [13] "ra_birds"                 "ra_elephant"             
## [15] "ra_monkeys"               "ra_rodent"               
## [17] "ra_ungulate"              "rich_all_species"        
## [19] "evenness_all_species"     "diversity_all_species"   
## [21] "rich_bird_species"        "evenness_bird_species"   
## [23] "diversity_bird_species"   "rich_mammal_species"     
## [25] "evenness_mammal_species"  "diversity_mammal_species"
```



```r
vertebrates$hunt_cat <- as.factor(vertebrates$hunt_cat)
class(vertebrates$hunt_cat)
```

```
## [1] "factor"
```


```r
vertebrates$land_use <- as.factor(vertebrates$land_use)
class(vertebrates$land_use)
```

```
## [1] "factor"
```


10. (4 points) For the transects with high and moderate hunting intensity, how does the average diversity of birds and mammals compare?

```r
view(vertebrates)
```



```r
vertebrates %>%
  filter(hunt_cat=="Moderate") %>% 
  summarize(bird_diversity=mean(diversity_bird_species),mammal_diversity=mean(diversity_mammal_species))
```

```
## # A tibble: 1 × 2
##   bird_diversity mammal_diversity
##            <dbl>            <dbl>
## 1           1.62             1.68
```


```r
vertebrates %>%
  filter(hunt_cat=="High") %>% 
  summarize(bird_diversity=mean(diversity_bird_species),mammal_diversity=mean(diversity_mammal_species))
```

```
## # A tibble: 1 × 2
##   bird_diversity mammal_diversity
##            <dbl>            <dbl>
## 1           1.66             1.74
```

Alternatively:

```r
vertebrates %>% 
  filter(hunt_cat=="Moderate") %>% 
  summarize(mean_bird_diversity=mean(diversity_bird_species),
            mean_mammal_diversity=mean(diversity_mammal_species),
            nsamples=n())
```

```
## # A tibble: 1 × 3
##   mean_bird_diversity mean_mammal_diversity nsamples
##                 <dbl>                 <dbl>    <int>
## 1                1.62                  1.68        8
```


11. (4 points) One of the conclusions in the study is that the relative abundance of animals drops off the closer you get to a village. Let's try to reconstruct this (without the statistics). 

How does the relative abundance (RA) of apes, birds, elephants, monkeys, rodents, and ungulates compare between sites that are less than 3km from a village to sites that are greater than 25km from a village? The variable `Distance` measures the distance of the transect from the nearest village. Hint: try using the `across` operator.  

Close

```r
vertebrates %>% 
  filter(distance<3) %>% 
  summarise(across(contains("ra_"), mean))
```

```
## # A tibble: 1 × 6
##   ra_apes ra_birds ra_elephant ra_monkeys ra_rodent ra_ungulate
##     <dbl>    <dbl>       <dbl>      <dbl>     <dbl>       <dbl>
## 1    0.12     76.6       0.145       17.3      3.90        1.87
```

Far

```r
vertebrates  %>% 
  filter(distance>25) %>% 
  summarise(across(contains("ra_"), mean))
```

```
## # A tibble: 1 × 6
##   ra_apes ra_birds ra_elephant ra_monkeys ra_rodent ra_ungulate
##     <dbl>    <dbl>       <dbl>      <dbl>     <dbl>       <dbl>
## 1    4.91     31.6           0       54.1      1.29        8.12
```

12. (4 points) Based on your interest, do one exploratory analysis on the `gabon` data of your choice. This analysis needs to include a minimum of two functions in `dplyr.`
