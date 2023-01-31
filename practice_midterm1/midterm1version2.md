---
title: "Midterm 1"
author: "Riya Jariwala"
date: "2023-01-31"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your code should be organized, clean, and run free from errors. Remember, you must remove the `#` for any included code chunks to run. Be sure to add your name to the author header above.  

After the first 50 minutes, please upload your code (5 points). During the second 50 minutes, you may get help from each other- but no copy/paste. Upload the last version at the end of this time, but be sure to indicate it as final. If you finish early, you are free to leave.

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean! Use the tidyverse and pipes unless otherwise indicated. This exam is worth a total of 35 points. 

Please load the following libraries.

```r
library(tidyverse)
```

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

```r
library(janitor)
```

```
## 
## Attaching package: 'janitor'
## 
## The following objects are masked from 'package:stats':
## 
##     chisq.test, fisher.test
```

In the midterm 1 folder there is a second folder called `data`. Inside the `data` folder, there is a .csv file called `ecs21351-sup-0003-SupplementS1.csv`. These data are from Soykan, C. U., J. Sauer, J. G. Schuetz, G. S. LeBaron, K. Dale, and G. M. Langham. 2016. Population trends for North American winter birds based on hierarchical models. Ecosphere 7(5):e01351. 10.1002/ecs2.1351.  

Please load these data as a new object called `ecosphere`. In this step, I am providing the code to load the data, clean the variable names, and remove a footer that the authors used as part of the original publication.

```r
ecosphere <- read_csv("data/ecs21351-sup-0003-SupplementS1.csv", skip=2) %>% 
  clean_names() %>%
  slice(1:(n() - 18)) # this removes the footer
```

Problem 1. (1 point) Let's start with some data exploration. What are the variable names?

```r
names(ecosphere)
```

```
##  [1] "order"                       "family"                     
##  [3] "common_name"                 "scientific_name"            
##  [5] "diet"                        "life_expectancy"            
##  [7] "habitat"                     "urban_affiliate"            
##  [9] "migratory_strategy"          "log10_mass"                 
## [11] "mean_eggs_per_clutch"        "mean_age_at_sexual_maturity"
## [13] "population_size"             "winter_range_area"          
## [15] "range_in_cbc"                "strata"                     
## [17] "circles"                     "feeder_bird"                
## [19] "median_trend"                "lower_95_percent_ci"        
## [21] "upper_95_percent_ci"
```

Problem 2. (1 point) Use the function of your choice to summarize the data.

```r
summary(ecosphere)
```

```
##     order              family          common_name        scientific_name   
##  Length:551         Length:551         Length:551         Length:551        
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##      diet           life_expectancy      habitat          urban_affiliate   
##  Length:551         Length:551         Length:551         Length:551        
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##  migratory_strategy   log10_mass    mean_eggs_per_clutch
##  Length:551         Min.   :0.480   Min.   : 1.000      
##  Class :character   1st Qu.:1.365   1st Qu.: 3.000      
##  Mode  :character   Median :1.890   Median : 4.000      
##                     Mean   :2.012   Mean   : 4.527      
##                     3rd Qu.:2.685   3rd Qu.: 5.000      
##                     Max.   :4.040   Max.   :17.000      
##                                                         
##  mean_age_at_sexual_maturity population_size     winter_range_area  
##  Min.   : 0.200              Min.   :    15000   Min.   :       11  
##  1st Qu.: 1.000              1st Qu.:  1100000   1st Qu.:   819357  
##  Median : 1.000              Median :  4900000   Median :  2189639  
##  Mean   : 1.592              Mean   : 18446745   Mean   :  5051047  
##  3rd Qu.: 2.000              3rd Qu.: 18000000   3rd Qu.:  6778598  
##  Max.   :12.500              Max.   :300000000   Max.   :185968946  
##                              NA's   :273                            
##   range_in_cbc        strata          circles       feeder_bird       
##  Min.   :  0.00   Min.   :  1.00   Min.   :   2.0   Length:551        
##  1st Qu.:  2.35   1st Qu.:  3.00   1st Qu.:  46.5   Class :character  
##  Median : 30.30   Median : 11.00   Median : 184.0   Mode  :character  
##  Mean   : 38.48   Mean   : 32.43   Mean   : 558.9                     
##  3rd Qu.: 72.95   3rd Qu.: 42.00   3rd Qu.: 661.0                     
##  Max.   :100.00   Max.   :159.00   Max.   :3202.0                     
##                                                                       
##   median_trend   lower_95_percent_ci upper_95_percent_ci
##  Min.   :0.739   Min.   :0.5780      Min.   :    0.798  
##  1st Qu.:0.993   1st Qu.:0.9675      1st Qu.:    1.011  
##  Median :1.009   Median :0.9930      Median :    1.027  
##  Mean   :1.016   Mean   :0.9857      Mean   :   33.709  
##  3rd Qu.:1.030   3rd Qu.:1.0140      3rd Qu.:    1.055  
##  Max.   :1.396   Max.   :1.3080      Max.   :18000.000  
## 
```

Problem 3. (2 points) How many distinct orders of birds are represented in the data?

```r
ecosphere %>% 
  summarize(distinct_orders=n_distinct(order))
```

```
## # A tibble: 1 × 1
##   distinct_orders
##             <int>
## 1              19
```
There are 19 distinct orders of birds in the data.

Problem 4. (2 points) Which habitat has the highest diversity (number of species) in the data?

```r
ecosphere %>% 
  select(habitat) %>% 
  count(habitat, sort=T) %>% 
  arrange(desc(habitat))
```

```
## # A tibble: 7 × 2
##   habitat       n
##   <chr>     <int>
## 1 Woodland    177
## 2 Wetland     153
## 3 Various      45
## 4 Shrubland    82
## 5 Ocean        44
## 6 Grassland    36
## 7 <NA>         14
```
The Woodland habitat has the highest number of species which is 177. 

Run the code below to learn about the `slice` function. Look specifically at the examples (at the bottom) for `slice_max()` and `slice_min()`. If you are still unsure, try looking up examples online (https://rpubs.com/techanswers88/dplyr-slice). Use this new function to answer question 5 below.

```r
?slice_max
```

Problem 5. (4 points) Using the `slice_max()` or `slice_min()` function described above which species has the largest and smallest winter range?

```r
ecosphere %>% 
  slice_min(winter_range_area)
```

```
## # A tibble: 1 × 21
##   order     family commo…¹ scien…² diet  life_…³ habitat urban…⁴ migra…⁵ log10…⁶
##   <chr>     <chr>  <chr>   <chr>   <chr> <chr>   <chr>   <chr>   <chr>     <dbl>
## 1 Passerif… Alaud… Skylark Alauda… Seed  Short   Grassl… No      Reside…    1.57
## # … with 11 more variables: mean_eggs_per_clutch <dbl>,
## #   mean_age_at_sexual_maturity <dbl>, population_size <dbl>,
## #   winter_range_area <dbl>, range_in_cbc <dbl>, strata <dbl>, circles <dbl>,
## #   feeder_bird <chr>, median_trend <dbl>, lower_95_percent_ci <dbl>,
## #   upper_95_percent_ci <dbl>, and abbreviated variable names ¹​common_name,
## #   ²​scientific_name, ³​life_expectancy, ⁴​urban_affiliate, ⁵​migratory_strategy,
## #   ⁶​log10_mass
```
The species Alauda arvensis has the smallest winter range.


```r
ecosphere %>% 
  slice_max(winter_range_area)
```

```
## # A tibble: 1 × 21
##   order     family commo…¹ scien…² diet  life_…³ habitat urban…⁴ migra…⁵ log10…⁶
##   <chr>     <chr>  <chr>   <chr>   <chr> <chr>   <chr>   <chr>   <chr>     <dbl>
## 1 Procella… Proce… Sooty … Puffin… Vert… Long    Ocean   No      Long        2.9
## # … with 11 more variables: mean_eggs_per_clutch <dbl>,
## #   mean_age_at_sexual_maturity <dbl>, population_size <dbl>,
## #   winter_range_area <dbl>, range_in_cbc <dbl>, strata <dbl>, circles <dbl>,
## #   feeder_bird <chr>, median_trend <dbl>, lower_95_percent_ci <dbl>,
## #   upper_95_percent_ci <dbl>, and abbreviated variable names ¹​common_name,
## #   ²​scientific_name, ³​life_expectancy, ⁴​urban_affiliate, ⁵​migratory_strategy,
## #   ⁶​log10_mass
```
The species Puffinus griseus has the largest winter range. 

Problem 6. (2 points) The family Anatidae includes ducks, geese, and swans. Make a new object `ducks` that only includes species in the family Anatidae. Restrict this new dataframe to include all variables except order and family.

```r
ducks <- ecosphere %>% 
  filter(family=="Anatidae") %>% 
  select(-order, -family)
ducks
```

```
## # A tibble: 44 × 19
##    commo…¹ scien…² diet  life_…³ habitat urban…⁴ migra…⁵ log10…⁶ mean_…⁷ mean_…⁸
##    <chr>   <chr>   <chr> <chr>   <chr>   <chr>   <chr>     <dbl>   <dbl>   <dbl>
##  1 "Ameri… Anas r… Vege… Long    Wetland No      Short      3.09     9       1  
##  2 "Ameri… Anas a… Vege… Middle  Wetland No      Short      2.88     7.5     1  
##  3 "Barro… Buceph… Inve… Middle  Wetland No      Modera…    2.96    10.5     3  
##  4 "Black… Branta… Vege… Long    Wetland No      Modera…    3.11     3.5     2.5
##  5 "Black… Melani… Inve… Middle  Wetland No      Modera…    3.02     9.5     2  
##  6 "Black… Dendro… Vege… Short   Wetland No      Withdr…    2.88    13.5     1  
##  7 "Blue-… Anas d… Vege… Middle  Wetland No      Modera…    2.56    10       0.6
##  8 "Buffl… Buceph… Inve… Middle  Wetland No      Short      2.6      8.5     2  
##  9 "Cackl… Branta… Vege… Middle  Wetland Yes     Short      3.45     5       1  
## 10 "Canva… Aythya… Vege… Middle  Wetland No      Short      3.08     8       1  
## # … with 34 more rows, 9 more variables: population_size <dbl>,
## #   winter_range_area <dbl>, range_in_cbc <dbl>, strata <dbl>, circles <dbl>,
## #   feeder_bird <chr>, median_trend <dbl>, lower_95_percent_ci <dbl>,
## #   upper_95_percent_ci <dbl>, and abbreviated variable names ¹​common_name,
## #   ²​scientific_name, ³​life_expectancy, ⁴​urban_affiliate, ⁵​migratory_strategy,
## #   ⁶​log10_mass, ⁷​mean_eggs_per_clutch, ⁸​mean_age_at_sexual_maturity
```

Problem 7. (2 points) We might assume that all ducks live in wetland habitat. Is this true for the ducks in these data? If there are exceptions, list the species below.

```r
ducks %>% 
filter(habitat!="Wetland")
```

```
## # A tibble: 1 × 19
##   common…¹ scien…² diet  life_…³ habitat urban…⁴ migra…⁵ log10…⁶ mean_…⁷ mean_…⁸
##   <chr>    <chr>   <chr> <chr>   <chr>   <chr>   <chr>     <dbl>   <dbl>   <dbl>
## 1 Common … Somate… Inve… Middle  Ocean   No      Short      3.31       5     2.5
## # … with 9 more variables: population_size <dbl>, winter_range_area <dbl>,
## #   range_in_cbc <dbl>, strata <dbl>, circles <dbl>, feeder_bird <chr>,
## #   median_trend <dbl>, lower_95_percent_ci <dbl>, upper_95_percent_ci <dbl>,
## #   and abbreviated variable names ¹​common_name, ²​scientific_name,
## #   ³​life_expectancy, ⁴​urban_affiliate, ⁵​migratory_strategy, ⁶​log10_mass,
## #   ⁷​mean_eggs_per_clutch, ⁸​mean_age_at_sexual_maturity
```

Problem 8. (4 points) In ducks, how is mean body mass associated with migratory strategy? Do the ducks that migrate long distances have high or low average body mass?

```r
ducks %>% 
  select(migratory_strategy, log10_mass) %>%
  group_by(migratory_strategy) %>% 
  summarize(average_log10_mass=mean(log10_mass)) %>% 
  arrange(average_log10_mass)
```

```
## # A tibble: 5 × 2
##   migratory_strategy average_log10_mass
##   <chr>                           <dbl>
## 1 Long                             2.87
## 2 Withdrawal                       2.92
## 3 Short                            2.98
## 4 Moderate                         3.11
## 5 Resident                         4.03
```
The ducks that migrate long distances have a low average body mass. 

Problem 9. (2 points) Accipitridae is the family that includes eagles, hawks, kites, and osprey. First, make a new object `eagles` that only includes species in the family Accipitridae. Next, restrict these data to only include the variables common_name, scientific_name, and population_size.

```r
eagles <- ecosphere %>% 
  filter(family=="Accipitridae") %>% 
  select(common_name, scientific_name, population_size)
eagles 
```

```
## # A tibble: 20 × 3
##    common_name         scientific_name          population_size
##    <chr>               <chr>                              <dbl>
##  1 Bald Eagle          Haliaeetus leucocephalus              NA
##  2 Broad-winged Hawk   Buteo platypterus                1700000
##  3 Cooper's Hawk       Accipiter cooperii                700000
##  4 Ferruginous Hawk    Buteo regalis                      80000
##  5 Golden Eagle        Aquila chrysaetos                 130000
##  6 Gray Hawk           Buteo nitidus                         NA
##  7 Harris's Hawk       Parabuteo unicinctus               50000
##  8 Hook-billed Kite    Chondrohierax uncinatus               NA
##  9 Northern Goshawk    Accipiter gentilis                200000
## 10 Northern Harrier    Circus cyaneus                    700000
## 11 Red-shouldered Hawk Buteo lineatus                   1100000
## 12 Red-tailed Hawk     Buteo jamaicensis                2000000
## 13 Rough-legged Hawk   Buteo lagopus                     300000
## 14 Sharp-shinned Hawk  Accipiter striatus                500000
## 15 Short-tailed Hawk   Buteo brachyurus                      NA
## 16 Snail Kite          Rostrhamus sociabilis                 NA
## 17 Swainson's Hawk     Buteo swainsoni                   540000
## 18 White-tailed Hawk   Buteo albicaudatus                    NA
## 19 White-tailed Kite   Elanus leucurus                       NA
## 20 Zone-tailed Hawk    Buteo albonotatus                     NA
```

Problem 10. (4 points) In the eagles data, any species with a population size less than 250,000 individuals is threatened. Make a new column `conservation_status` that shows whether or not a species is threatened.

```r
conservation_status <- c(eagles$population_size<250000)
eagles %>% mutate(conservation_status)
```

```
## # A tibble: 20 × 4
##    common_name         scientific_name          population_size conservation_s…¹
##    <chr>               <chr>                              <dbl> <lgl>           
##  1 Bald Eagle          Haliaeetus leucocephalus              NA NA              
##  2 Broad-winged Hawk   Buteo platypterus                1700000 FALSE           
##  3 Cooper's Hawk       Accipiter cooperii                700000 FALSE           
##  4 Ferruginous Hawk    Buteo regalis                      80000 TRUE            
##  5 Golden Eagle        Aquila chrysaetos                 130000 TRUE            
##  6 Gray Hawk           Buteo nitidus                         NA NA              
##  7 Harris's Hawk       Parabuteo unicinctus               50000 TRUE            
##  8 Hook-billed Kite    Chondrohierax uncinatus               NA NA              
##  9 Northern Goshawk    Accipiter gentilis                200000 TRUE            
## 10 Northern Harrier    Circus cyaneus                    700000 FALSE           
## 11 Red-shouldered Hawk Buteo lineatus                   1100000 FALSE           
## 12 Red-tailed Hawk     Buteo jamaicensis                2000000 FALSE           
## 13 Rough-legged Hawk   Buteo lagopus                     300000 FALSE           
## 14 Sharp-shinned Hawk  Accipiter striatus                500000 FALSE           
## 15 Short-tailed Hawk   Buteo brachyurus                      NA NA              
## 16 Snail Kite          Rostrhamus sociabilis                 NA NA              
## 17 Swainson's Hawk     Buteo swainsoni                   540000 FALSE           
## 18 White-tailed Hawk   Buteo albicaudatus                    NA NA              
## 19 White-tailed Kite   Elanus leucurus                       NA NA              
## 20 Zone-tailed Hawk    Buteo albonotatus                     NA NA              
## # … with abbreviated variable name ¹​conservation_status
```
To clarify for the conservation status column, TRUE means that the population is threatened and FALSE means that it is not threatened. 

Problem 11. (2 points) Consider the results from questions 9 and 10. Are there any species for which their threatened status needs further study? How do you know?

```r
eagles %>% 
  filter(conservation_status)
```

```
## # A tibble: 4 × 3
##   common_name      scientific_name      population_size
##   <chr>            <chr>                          <dbl>
## 1 Ferruginous Hawk Buteo regalis                  80000
## 2 Golden Eagle     Aquila chrysaetos             130000
## 3 Harris's Hawk    Parabuteo unicinctus           50000
## 4 Northern Goshawk Accipiter gentilis            200000
```

```r
is.na(conservation_status)
```

```
##  [1]  TRUE FALSE FALSE FALSE FALSE  TRUE FALSE  TRUE FALSE FALSE FALSE FALSE
## [13] FALSE FALSE  TRUE  TRUE FALSE  TRUE  TRUE  TRUE
```
Yes there are 8 species for which their threatened status needs further study because the conservation status for those species return "TRUE" for NA. 

Problem 12. (4 points) Use the `ecosphere` data to perform one exploratory analysis of your choice. The analysis must have a minimum of three lines and two functions. You must also clearly state the question you are attempting to answer.

Question: Within the Alcidae family, what is the most common type of migration strategy of the smallest  animals (log10 mass of less than 2.5)?


```r
ecosphere %>% 
  select(family, migratory_strategy, log10_mass) %>% 
  filter(family=="Alcidae") %>% 
  group_by(migratory_strategy) %>% 
  arrange(log10_mass)
```

```
## # A tibble: 11 × 3
## # Groups:   migratory_strategy [2]
##    family  migratory_strategy log10_mass
##    <chr>   <chr>                   <dbl>
##  1 Alcidae Withdrawal               2.26
##  2 Alcidae Short                    2.27
##  3 Alcidae Short                    2.34
##  4 Alcidae Withdrawal               2.34
##  5 Alcidae Withdrawal               2.58
##  6 Alcidae Withdrawal               2.68
##  7 Alcidae Withdrawal               2.72
##  8 Alcidae Short                    2.77
##  9 Alcidae Short                    2.86
## 10 Alcidae Short                    2.98
## 11 Alcidae Short                    3
```
Short distances and withdrawal are the most common migratory strategies for animals in the Alcidae family. Small animals of this family either migrate short distances or withdraw (the frequency of these strategies are equal). 

Please provide the names of the students you have worked with with during the exam: I worked with Jaishree and Katie. 


```r
ecosphere %>% 
  select(family, migratory_strategy, log10_mass) %>% 
  filter(family=="Alcidae") %>% 
  group_by(migratory_strategy) %>% 
  summarize(log10_mass)
```

```
## `summarise()` has grouped output by 'migratory_strategy'. You can override
## using the `.groups` argument.
```

```
## # A tibble: 11 × 2
## # Groups:   migratory_strategy [2]
##    migratory_strategy log10_mass
##    <chr>                   <dbl>
##  1 Short                    2.34
##  2 Short                    2.77
##  3 Short                    2.27
##  4 Short                    3   
##  5 Short                    2.86
##  6 Short                    2.98
##  7 Withdrawal               2.58
##  8 Withdrawal               2.26
##  9 Withdrawal               2.34
## 10 Withdrawal               2.72
## 11 Withdrawal               2.68
```

Please be 100% sure your exam is saved, knitted, and pushed to your github repository. No need to submit a link on canvas, we will find your exam in your repository.
