---
title: "Lab 4 Intro"
date: "2023-01-19"
output: 
  ioslides_presentation: 
    keep_md: yes
---



## Setup
1. Login to the lab computer (please don't use your personal computer).  
2. Navigate to github.com and login.   
2. Copy your repository to the desktop.   
5. Copy the class repository to the desktop (https://github.com/jmledford3115/datascibiol).  
6. Copy the files for today's lab from the class repository and paste them into **your** repository.  
7. Open today's lab in RStudio.  

## Review from last time
### *With a partner, discuss the following questions*
1. How is a data frame and how is it different than a data matrix?  
##Data frames can store multiple classes of data while data matrices cannot?
creates spreadsheet, format is cleaner and creates a spreadsheet 
2. What is the command we use to import .csv files into R? 
##hot_springs <- readr::read_csv("hsprings_data.csv")
3. Take a moment to show your repository on GitHub to a partner. Can you make any improvements?  

## Warm-up
1. In a new R Markdown document, load the tidyverse and a package called `palmerpenguins`.
2. What are the dimensions of the dataset `penguins`?
3. What are the names of the variables in the `penguins` dataset?
4. How many individuals were sampled on each island in the `penguins` dataset?
5. What is the mean body mass for all individuals in the `penguins` dataset?


```
## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.2 ──
## ✔ ggplot2 3.4.0      ✔ purrr   1.0.0 
## ✔ tibble  3.1.8      ✔ dplyr   1.0.10
## ✔ tidyr   1.2.1      ✔ stringr 1.5.0 
## ✔ readr   2.1.3      ✔ forcats 0.5.2 
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
```


```
## [1] 344   8
```

```
## [1] "species"           "island"            "bill_length_mm"   
## [4] "bill_depth_mm"     "flipper_length_mm" "body_mass_g"      
## [7] "sex"               "year"
```
1 individual sampled


```
## [1] 4201.754
```


```
##       species          island    bill_length_mm  bill_depth_mm  
##  Adelie   :152   Biscoe   :168   Min.   :32.10   Min.   :13.10  
##  Chinstrap: 68   Dream    :124   1st Qu.:39.23   1st Qu.:15.60  
##  Gentoo   :124   Torgersen: 52   Median :44.45   Median :17.30  
##                                  Mean   :43.92   Mean   :17.15  
##                                  3rd Qu.:48.50   3rd Qu.:18.70  
##                                  Max.   :59.60   Max.   :21.50  
##                                  NA's   :2       NA's   :2      
##  flipper_length_mm  body_mass_g       sex           year     
##  Min.   :172.0     Min.   :2700   female:165   Min.   :2007  
##  1st Qu.:190.0     1st Qu.:3550   male  :168   1st Qu.:2007  
##  Median :197.0     Median :4050   NA's  : 11   Median :2008  
##  Mean   :200.9     Mean   :4202                Mean   :2008  
##  3rd Qu.:213.0     3rd Qu.:4750                3rd Qu.:2009  
##  Max.   :231.0     Max.   :6300                Max.   :2009  
##  NA's   :2         NA's   :2
```

