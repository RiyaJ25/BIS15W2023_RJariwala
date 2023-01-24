---
title: "Lab 5 Intro Redone"
output: 
  html_document: 
    keep_md: yes
date: "2023-01-24"
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
1. What are the characteristics of `tidy` data?  
- collection of packages that some data scientist made, easier to analyze data in R by just loading the whole tidy verse
- it's easier to use Rmd with these packages instead of base R commands
- every variable has own column, every observation has own row, every cell has a unique value 

2. What is the difference between `select` and `filter`? 
With select you can look at columns of a data frame and with filter you can look at the specific values that correspond to a certain column

3. When is your first midterm?  
A week from today!

## Warm-up
1. Load the bison data.

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
## Rows: 8325 Columns: 8
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (3): data_code, animal_code, animal_sex
## dbl (5): rec_year, rec_month, rec_day, animal_weight, animal_yob
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

2. What are the dimesions and structure of the data?

```
## [1] 8325    8
```

```
## spc_tbl_ [8,325 × 8] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##  $ data_code    : chr [1:8325] "CBH01" "CBH01" "CBH01" "CBH01" ...
##  $ rec_year     : num [1:8325] 1994 1994 1994 1994 1994 ...
##  $ rec_month    : num [1:8325] 11 11 11 11 11 11 11 11 11 11 ...
##  $ rec_day      : num [1:8325] 8 8 8 8 8 8 8 8 8 8 ...
##  $ animal_code  : chr [1:8325] "813" "834" "B-301" "B-402" ...
##  $ animal_sex   : chr [1:8325] "F" "F" "F" "F" ...
##  $ animal_weight: num [1:8325] 890 1074 1060 989 1062 ...
##  $ animal_yob   : num [1:8325] 1981 1983 1983 1984 1984 ...
##  - attr(*, "spec")=
##   .. cols(
##   ..   data_code = col_character(),
##   ..   rec_year = col_double(),
##   ..   rec_month = col_double(),
##   ..   rec_day = col_double(),
##   ..   animal_code = col_character(),
##   ..   animal_sex = col_character(),
##   ..   animal_weight = col_double(),
##   ..   animal_yob = col_double()
##   .. )
##  - attr(*, "problems")=<externalptr>
```

3. We are only interested in code, sex, weight, year of birth. Restrict the data to these variables and store the dataframe as a new object.

```
## [1] "data_code"     "rec_year"      "rec_month"     "rec_day"      
## [5] "animal_code"   "animal_sex"    "animal_weight" "animal_yob"
```


```
## # A tibble: 8,325 × 4
##    animal_code animal_sex animal_weight animal_yob
##    <chr>       <chr>              <dbl>      <dbl>
##  1 813         F                    890       1981
##  2 834         F                   1074       1983
##  3 B-301       F                   1060       1983
##  4 B-402       F                    989       1984
##  5 B-403       F                   1062       1984
##  6 B-502       F                    978       1985
##  7 B-503       F                   1068       1985
##  8 B-504       F                   1024       1985
##  9 B-601       F                    978       1986
## 10 B-602       F                   1188       1986
## # … with 8,315 more rows
```

4. Pull out the animals born between 1980-1990.

```
## # A tibble: 435 × 4
##    animal_code animal_sex animal_weight animal_yob
##    <chr>       <chr>              <dbl>      <dbl>
##  1 813         F                    890       1981
##  2 834         F                   1074       1983
##  3 B-301       F                   1060       1983
##  4 B-402       F                    989       1984
##  5 B-403       F                   1062       1984
##  6 B-502       F                    978       1985
##  7 B-503       F                   1068       1985
##  8 B-504       F                   1024       1985
##  9 B-601       F                    978       1986
## 10 B-602       F                   1188       1986
## # … with 425 more rows
```

5. How many male and female bison are represented between 1980-1990?

```
## # A tibble: 414 × 4
##    animal_code animal_sex animal_weight animal_yob
##    <chr>       <chr>              <dbl>      <dbl>
##  1 813         F                    890       1981
##  2 834         F                   1074       1983
##  3 B-301       F                   1060       1983
##  4 B-402       F                    989       1984
##  5 B-403       F                   1062       1984
##  6 B-502       F                    978       1985
##  7 B-503       F                   1068       1985
##  8 B-504       F                   1024       1985
##  9 B-601       F                    978       1986
## 10 B-602       F                   1188       1986
## # … with 404 more rows
```


```
## # A tibble: 21 × 4
##    animal_code animal_sex animal_weight animal_yob
##    <chr>       <chr>              <dbl>      <dbl>
##  1 108         M                   1728       1987
##  2 888         M                   1726       1988
##  3 88Q         M                   1712       1988
##  4 892         M                   1306       1989
##  5 89F         M                   1682       1989
##  6 89N         M                   1594       1989
##  7 904         M                   1552       1990
##  8 905         M                   1572       1990
##  9 908         M                   1538       1990
## 10 90L         M                   1422       1990
## # … with 11 more rows
```
414 females and 21 males 

6. Between 1980-1990, were males or females larger on average?

```
## [1] 1017.314
```


```
## [1] 1543.333
```
Males have a larger weight on average 

#class notes
bison_yob <- filter(bison_new, animal_yob>=1980 & animal_yob,=1990)


table(bison_yob$animal_sex)


then use male female filter animal sex =="M"


then find means 
