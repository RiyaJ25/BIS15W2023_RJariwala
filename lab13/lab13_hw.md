---
title: "Lab 13 Homework"
author: "Riya Jariwala"
date: "2023-03-07"
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
library(ggmap)
library(ggplot2)
```


```r
#library(albersusa)
```

## Load the Data
We will use two separate data sets for this homework.  

1. The first [data set](https://rcweb.dartmouth.edu/~f002d69/workshops/index_rspatial.html) represent sightings of grizzly bears (Ursos arctos) in Alaska.  

2. The second data set is from Brandell, Ellen E (2021), Serological dataset and R code for: Patterns and processes of pathogen exposure in gray wolves across North America, Dryad, [Dataset](https://doi.org/10.5061/dryad.5hqbzkh51).  

1. Load the `grizzly` data and evaluate its structure.  

```r
bears <- read_csv(here("data", "bear-sightings.csv"))
```

```
## Rows: 494 Columns: 3
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## dbl (3): bear.id, longitude, latitude
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```


2. Use the range of the latitude and longitude to build an appropriate bounding box for your map.  

```r
bears %>% 
  select(latitude, longitude) %>% 
  summary()
```

```
##     latitude       longitude     
##  Min.   :55.02   Min.   :-166.2  
##  1st Qu.:58.13   1st Qu.:-154.2  
##  Median :60.97   Median :-151.0  
##  Mean   :61.41   Mean   :-149.1  
##  3rd Qu.:64.13   3rd Qu.:-145.6  
##  Max.   :70.37   Max.   :-131.3
```

```r
lat <- c(55.02, 70.37)
long <- c(-166.2, -131.3)
bbox <- make_bbox(long, lat, f = 0.05)
```

3. Load a map from `stamen` in a terrain style projection and display the map.  

```r
bear_map <- get_map(bbox, maptype = "terrain", source = "stamen")
```

```
## ℹ Map tiles by Stamen Design, under CC BY 3.0. Data by OpenStreetMap, under ODbL.
```

  
4. Build a final map that overlays the recorded observations of grizzly bears in Alaska.  

```r
ggmap(bear_map) + 
  geom_point(data = bears, aes(longitude, latitude), color="brown", size=0.5) +
  labs(x= "Longitude", y= "Latitude", title="Bear Locations")
```

![](lab13_hw_files/figure-html/unnamed-chunk-7-1.png)<!-- -->


Let's switch to the wolves data. Brandell, Ellen E (2021), Serological dataset and R code for: Patterns and processes of pathogen exposure in gray wolves across North America, Dryad, [Dataset](https://doi.org/10.5061/dryad.5hqbzkh51).  

5. Load the data and evaluate its structure.  

```r
wolves <- read_csv(here("data", "wolves_data", "wolves_dataset.csv"))
```

```
## Rows: 1986 Columns: 23
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr  (4): pop, age.cat, sex, color
## dbl (19): year, lat, long, habitat, human, pop.density, pack.size, standard....
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```


6. How many distinct wolf populations are included in this study? Mae a new object that restricts the data to the wolf populations in the lower 48 US states.  

```r
wolves %>% 
  select(pop) %>% 
  summarize(n_pops=n_distinct(pop))
```

```
## # A tibble: 1 × 1
##   n_pops
##    <int>
## 1     17
```

```r
lowerUS_pops <- wolves %>% 
  filter(lat<=49)
lowerUS_pops
```

```
## # A tibble: 1,169 × 23
##    pop    year age.cat sex   color   lat  long habitat human pop.density pack.…¹
##    <chr> <dbl> <chr>   <chr> <chr> <dbl> <dbl>   <dbl> <dbl>       <dbl>   <dbl>
##  1 GTNP   2012 P       M     G      43.8 -111.  10375. 3924.        34.0     8.1
##  2 GTNP   2012 P       F     G      43.8 -111.  10375. 3924.        34.0     8.1
##  3 GTNP   2012 P       F     G      43.8 -111.  10375. 3924.        34.0     8.1
##  4 GTNP   2012 P       M     B      43.8 -111.  10375. 3924.        34.0     8.1
##  5 GTNP   2013 A       F     G      43.8 -111.  10375. 3924.        34.0     8.1
##  6 GTNP   2013 A       M     G      43.8 -111.  10375. 3924.        34.0     8.1
##  7 GTNP   2013 P       M     G      43.8 -111.  10375. 3924.        34.0     8.1
##  8 GTNP   2013 P       M     G      43.8 -111.  10375. 3924.        34.0     8.1
##  9 GTNP   2013 P       M     G      43.8 -111.  10375. 3924.        34.0     8.1
## 10 GTNP   2013 P       F     G      43.8 -111.  10375. 3924.        34.0     8.1
## # … with 1,159 more rows, 12 more variables: standard.habitat <dbl>,
## #   standard.human <dbl>, standard.pop <dbl>, standard.packsize <dbl>,
## #   standard.latitude <dbl>, standard.longitude <dbl>, cav.binary <dbl>,
## #   cdv.binary <dbl>, cpv.binary <dbl>, chv.binary <dbl>, neo.binary <dbl>,
## #   toxo.binary <dbl>, and abbreviated variable name ¹​pack.size
```


7. Use the range of the latitude and longitude to build an appropriate bounding box for your map.  

```r
lowerUS_pops %>%
  select(lat, long) %>% 
  summary()
```

```
##       lat             long        
##  Min.   :33.89   Min.   :-110.99  
##  1st Qu.:44.60   1st Qu.:-110.99  
##  Median :44.60   Median :-110.55  
##  Mean   :43.95   Mean   :-106.91  
##  3rd Qu.:46.83   3rd Qu.:-109.17  
##  Max.   :47.75   Max.   : -86.82
```


```r
lat <- c(33.89, 47.75)
long <- c(-110.99, -86.82)
bbox_wolves <- make_bbox(long, lat, f = 0.05)
```

8.  Load a map from `stamen` in a `terrain-lines` projection and display the map.  

```r
wolf_map <- get_map(bbox_wolves, maptype = "terrain-lines", source = "stamen")
```

```
## ℹ Map tiles by Stamen Design, under CC BY 3.0. Data by OpenStreetMap, under ODbL.
```


```r
ggmap(wolf_map)
```

![](lab13_hw_files/figure-html/unnamed-chunk-14-1.png)<!-- -->


9. Build a final map that overlays the recorded observations of wolves in the lower 48 states.  

```r
ggmap(wolf_map) + 
  geom_point(data = lowerUS_pops, aes(long, lat)) +
  labs(x= "Longitude", y= "Latitude", title="Bear Locations in Lower 49", color="brown", size=0.5)
```

![](lab13_hw_files/figure-html/unnamed-chunk-15-1.png)<!-- -->


10. Use the map from #9 above, but add some aesthetics. Try to `fill` and `color` by population.  

```r
ggmap(wolf_map) + 
  geom_point(data = lowerUS_pops, aes(long, lat), color="red") +
  labs(x= "Longitude", y= "Latitude", title="Bear Locations in Lower 49")
```

![](lab13_hw_files/figure-html/unnamed-chunk-16-1.png)<!-- -->


## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences. 
