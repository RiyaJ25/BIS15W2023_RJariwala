---
title: "Lab 9 Homework"
author: "Riya Jariwala"
date: "2023-02-14"
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
library(here)
library(naniar)
```

For this homework, we will take a departure from biological data and use data about California colleges. These data are a subset of the national college scorecard (https://collegescorecard.ed.gov/data/). Load the `ca_college_data.csv` as a new object called `colleges`.

```r
colleges <- read_csv(here("data", "ca_college_data.csv"))
```

```
## Rows: 341 Columns: 10
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (4): INSTNM, CITY, STABBR, ZIP
## dbl (6): ADM_RATE, SAT_AVG, PCIP26, COSTT4_A, C150_4_POOLED, PFTFTUG1_EF
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

The variables are a bit hard to decipher, here is a key:  

INSTNM: Institution name  
CITY: California city  
STABBR: Location state  
ZIP: Zip code  
ADM_RATE: Admission rate  
SAT_AVG: SAT average score  
PCIP26: Percentage of degrees awarded in Biological And Biomedical Sciences  
COSTT4_A: Annual cost of attendance  
C150_4_POOLED: 4-year completion rate  
PFTFTUG1_EF: Percentage of undergraduate students who are first-time, full-time degree/certificate-seeking undergraduate students  

1. Use your preferred function(s) to have a look at the data and get an idea of its structure. Make sure you summarize NA's and determine whether or not the data are tidy. You may also consider dealing with any naming issues.

```r
naniar::miss_var_summary(colleges)
```

```
## # A tibble: 10 × 3
##    variable      n_miss pct_miss
##    <chr>          <int>    <dbl>
##  1 SAT_AVG          276     80.9
##  2 ADM_RATE         240     70.4
##  3 C150_4_POOLED    221     64.8
##  4 COSTT4_A         124     36.4
##  5 PFTFTUG1_EF       53     15.5
##  6 PCIP26            35     10.3
##  7 INSTNM             0      0  
##  8 CITY               0      0  
##  9 STABBR             0      0  
## 10 ZIP                0      0
```


```r
colleges_tidy<- clean_names(colleges)
colleges_tidy
```

```
## # A tibble: 341 × 10
##    instnm      city  stabbr zip   adm_r…¹ sat_avg pcip26 costt…² c150_…³ pftft…⁴
##    <chr>       <chr> <chr>  <chr>   <dbl>   <dbl>  <dbl>   <dbl>   <dbl>   <dbl>
##  1 Grossmont … El C… CA     9202…      NA      NA 0.0016    7956  NA       0.355
##  2 College of… Visa… CA     9327…      NA      NA 0.0066    8109  NA       0.541
##  3 College of… San … CA     9440…      NA      NA 0.0038    8278  NA       0.357
##  4 Ventura Co… Vent… CA     9300…      NA      NA 0.0035    8407  NA       0.382
##  5 Oxnard Col… Oxna… CA     9303…      NA      NA 0.0085    8516  NA       0.275
##  6 Moorpark C… Moor… CA     9302…      NA      NA 0.0151    8577  NA       0.429
##  7 Skyline Co… San … CA     9406…      NA      NA 0         8580   0.233   0.231
##  8 Glendale C… Glen… CA     9120…      NA      NA 0.002     9181  NA       0.421
##  9 Citrus Col… Glen… CA     9174…      NA      NA 0.0021    9281  NA       0.440
## 10 Fresno Cit… Fres… CA     93741      NA      NA 0.0324    9370  NA       0.366
## # … with 331 more rows, and abbreviated variable names ¹​adm_rate, ²​costt4_a,
## #   ³​c150_4_pooled, ⁴​pftftug1_ef
```

2. Which cities in California have the highest number of colleges?

```r
colleges_tidy %>% 
  select("city", "instnm") %>% 
  group_by(city) %>% 
  summarize(n_colleges=n_distinct(instnm)) %>% 
  arrange(desc(n_colleges))
```

```
## # A tibble: 161 × 2
##    city          n_colleges
##    <chr>              <int>
##  1 Los Angeles           24
##  2 San Diego             18
##  3 San Francisco         15
##  4 Sacramento            10
##  5 Berkeley               9
##  6 Oakland                9
##  7 Claremont              7
##  8 Pasadena               6
##  9 Fresno                 5
## 10 Irvine                 5
## # … with 151 more rows
```
LA has the most collges in California. 

3. Based on your answer to #2, make a plot that shows the number of colleges in the top 10 cities.

```r
colleges_tidy %>% 
  select("city", "instnm") %>% 
  group_by(city) %>% 
  summarize(n_colleges=n_distinct(instnm)) %>% 
  arrange(desc(n_colleges)) %>% 
  head(n=10) %>% 
  ggplot(aes(y=city, x=n_colleges))+geom_col()
```

![](lab9_hw_files/figure-html/unnamed-chunk-6-1.png)<!-- -->


4. The column `COSTT4_A` is the annual cost of each institution. Which city has the highest average cost? Where is it located?

```r
colleges_tidy %>% 
  select("city", "instnm", "costt4_a") %>% 
  group_by(city, instnm) %>% 
  mutate(mean_cost=mean(costt4_a)) %>% 
  arrange(desc(mean_cost))
```

```
## # A tibble: 341 × 4
## # Groups:   city, instnm [341]
##    city          instnm                                        costt4_a mean_c…¹
##    <chr>         <chr>                                            <dbl>    <dbl>
##  1 Claremont     Harvey Mudd College                              69355    69355
##  2 Los Angeles   Southern California Institute of Architecture    67225    67225
##  3 Los Angeles   University of Southern California                67064    67064
##  4 Los Angeles   Occidental College                               67046    67046
##  5 Claremont     Claremont McKenna College                        66325    66325
##  6 Malibu        Pepperdine University                            66152    66152
##  7 Claremont     Scripps College                                  66060    66060
##  8 Claremont     Pitzer College                                   65880    65880
##  9 San Francisco San Francisco Art Institute                      65453    65453
## 10 Claremont     Pomona College                                   64870    64870
## # … with 331 more rows, and abbreviated variable name ¹​mean_cost
```
Claremont has the highest mean cost. 

5. Based on your answer to #4, make a plot that compares the cost of the individual colleges in the most expensive city. Bonus! Add UC Davis here to see how it compares :>).

```r
colleges_tidy %>% 
  select("city", "instnm", "costt4_a") %>% 
  filter(city=="Claremont") %>% 
  group_by(instnm) %>% 
  ggplot(aes(y=instnm, x=costt4_a))+geom_col()
```

```
## Warning: Removed 2 rows containing missing values (`position_stack()`).
```

![](lab9_hw_files/figure-html/unnamed-chunk-8-1.png)<!-- -->

6. The column `ADM_RATE` is the admissions rate by college and `C150_4_POOLED` is the four-year completion rate. Use a scatterplot to show the relationship between these two variables. What do you think this means?

```r
ggplot(colleges_tidy, mapping=aes(x=adm_rate, y=c150_4_pooled))+geom_point()
```

```
## Warning: Removed 251 rows containing missing values (`geom_point()`).
```

![](lab9_hw_files/figure-html/unnamed-chunk-9-1.png)<!-- -->
There is a negative relationship between these variables. 

7. Is there a relationship between cost and four-year completion rate? (You don't need to do the stats, just produce a plot). What do you think this means?

```r
ggplot(colleges_tidy, mapping=aes(x=costt4_a, y=c150_4_pooled))+geom_point()
```

```
## Warning: Removed 225 rows containing missing values (`geom_point()`).
```

![](lab9_hw_files/figure-html/unnamed-chunk-10-1.png)<!-- -->
There is a slight positive correlation between these variables. 

8. The column titled `INSTNM` is the institution name. We are only interested in the University of California colleges. Make a new data frame that is restricted to UC institutions. You can remove `Hastings College of Law` and `UC San Francisco` as we are only interested in undergraduate institutions.

```r
uc_schools <- colleges_tidy %>% 
  filter_all(any_vars(str_detect(., pattern = "University of California")))
```

Remove `Hastings College of Law` and `UC San Francisco` and store the final data frame as a new object `univ_calif_final`.

```r
univ_calif_final <- uc_schools %>% 
   filter(instnm!="Hastings College of Law", instnm!="UC San Francisco")
univ_calif_final
```

```
## # A tibble: 10 × 10
##    instnm      city  stabbr zip   adm_r…¹ sat_avg pcip26 costt…² c150_…³ pftft…⁴
##    <chr>       <chr> <chr>  <chr>   <dbl>   <dbl>  <dbl>   <dbl>   <dbl>   <dbl>
##  1 University… La J… CA     92093   0.357    1324  0.216   31043   0.872   0.662
##  2 University… Irvi… CA     92697   0.406    1206  0.107   31198   0.876   0.725
##  3 University… Rive… CA     92521   0.663    1078  0.149   31494   0.73    0.811
##  4 University… Los … CA     9009…   0.180    1334  0.155   33078   0.911   0.661
##  5 University… Davis CA     9561…   0.423    1218  0.198   33904   0.850   0.605
##  6 University… Sant… CA     9506…   0.578    1201  0.193   34608   0.776   0.786
##  7 University… Berk… CA     94720   0.169    1422  0.105   34924   0.916   0.709
##  8 University… Sant… CA     93106   0.358    1281  0.108   34998   0.816   0.708
##  9 University… San … CA     9410…  NA          NA NA          NA  NA      NA    
## 10 University… San … CA     9414…  NA          NA NA          NA  NA      NA    
## # … with abbreviated variable names ¹​adm_rate, ²​costt4_a, ³​c150_4_pooled,
## #   ⁴​pftftug1_ef
```

Use `separate()` to separate institution name into two new columns "UNIV" and "CAMPUS".

```r
univ_calif_final %>% 
  separate(instnm, into= c("univ", "campus"), sep = "-")
```

```
## # A tibble: 10 × 11
##    univ         campus city  stabbr zip   adm_r…¹ sat_avg pcip26 costt…² c150_…³
##    <chr>        <chr>  <chr> <chr>  <chr>   <dbl>   <dbl>  <dbl>   <dbl>   <dbl>
##  1 University … San D… La J… CA     92093   0.357    1324  0.216   31043   0.872
##  2 University … Irvine Irvi… CA     92697   0.406    1206  0.107   31198   0.876
##  3 University … River… Rive… CA     92521   0.663    1078  0.149   31494   0.73 
##  4 University … Los A… Los … CA     9009…   0.180    1334  0.155   33078   0.911
##  5 University … Davis  Davis CA     9561…   0.423    1218  0.198   33904   0.850
##  6 University … Santa… Sant… CA     9506…   0.578    1201  0.193   34608   0.776
##  7 University … Berke… Berk… CA     94720   0.169    1422  0.105   34924   0.916
##  8 University … Santa… Sant… CA     93106   0.358    1281  0.108   34998   0.816
##  9 University … Hasti… San … CA     9410…  NA          NA NA          NA  NA    
## 10 University … San F… San … CA     9414…  NA          NA NA          NA  NA    
## # … with 1 more variable: pftftug1_ef <dbl>, and abbreviated variable names
## #   ¹​adm_rate, ²​costt4_a, ³​c150_4_pooled
```

9. The column `ADM_RATE` is the admissions rate by campus. Which UC has the lowest and highest admissions rates? Produce a numerical summary and an appropriate plot.

```r
univ_calif_final %>% 
  select(instnm, adm_rate) %>% 
  group_by(instnm) %>% 
  arrange(desc(adm_rate)) 
```

```
## # A tibble: 10 × 2
## # Groups:   instnm [10]
##    instnm                                           adm_rate
##    <chr>                                               <dbl>
##  1 University of California-Riverside                  0.663
##  2 University of California-Santa Cruz                 0.578
##  3 University of California-Davis                      0.423
##  4 University of California-Irvine                     0.406
##  5 University of California-Santa Barbara              0.358
##  6 University of California-San Diego                  0.357
##  7 University of California-Los Angeles                0.180
##  8 University of California-Berkeley                   0.169
##  9 University of California-Hastings College of Law   NA    
## 10 University of California-San Francisco             NA
```


```r
univ_calif_final %>% 
  select(instnm, adm_rate) %>% 
  group_by(instnm) %>% 
  arrange(desc(adm_rate)) %>% 
  ggplot(aes(y=instnm, x=adm_rate))+geom_col()
```

```
## Warning: Removed 2 rows containing missing values (`position_stack()`).
```

![](lab9_hw_files/figure-html/unnamed-chunk-15-1.png)<!-- -->

10. If you wanted to get a degree in biological or biomedical sciences, which campus confers the majority of these degrees? Produce a numerical summary and an appropriate plot.

```r
univ_calif_final %>% 
  select(instnm, pcip26) %>% 
  group_by(instnm) %>% 
  arrange(desc(pcip26))
```

```
## # A tibble: 10 × 2
## # Groups:   instnm [10]
##    instnm                                           pcip26
##    <chr>                                             <dbl>
##  1 University of California-San Diego                0.216
##  2 University of California-Davis                    0.198
##  3 University of California-Santa Cruz               0.193
##  4 University of California-Los Angeles              0.155
##  5 University of California-Riverside                0.149
##  6 University of California-Santa Barbara            0.108
##  7 University of California-Irvine                   0.107
##  8 University of California-Berkeley                 0.105
##  9 University of California-Hastings College of Law NA    
## 10 University of California-San Francisco           NA
```


```r
univ_calif_final %>% 
  select(instnm, pcip26) %>% 
  group_by(instnm) %>% 
  arrange(desc(pcip26)) %>% 
  ggplot(aes(y=instnm, x=pcip26))+geom_col()
```

```
## Warning: Removed 2 rows containing missing values (`position_stack()`).
```

![](lab9_hw_files/figure-html/unnamed-chunk-17-1.png)<!-- -->

## Knit Your Output and Post to [GitHub](https://github.com/FRS417-DataScienceBiologists)
