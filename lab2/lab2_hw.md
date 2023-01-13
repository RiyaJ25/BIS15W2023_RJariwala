---
title: "Lab 2 Homework"
author: "Riya Jariwala"
date: "2023-01-12"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
  pdf_document:
    toc: no
---
## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  
1. What is a vector in R?  
A vector in R is created using the "c" command, which stands for concatenate. Vectors are useful for organizing data in R and they can be used as numerical or character data. 
2. What is a data matrix in R?  
A data matrix in R can be constructed using the matrix, nrow, and byrow commands. They are stacked vectors and thus they're similar to data tables.  
3. Below are data collected by three scientists (Jill, Steve, Susan in order) measuring temperatures of eight hot springs. Run this code chunk to create the vectors.  

```r
spring_1 <- c(36.25, 35.40, 35.30)
spring_2 <- c(35.15, 35.35, 33.35)
spring_3 <- c(30.70, 29.65, 29.20)
spring_4 <- c(39.70, 40.05, 38.65)
spring_5 <- c(31.85, 31.40, 29.30)
spring_6 <- c(30.20, 30.65, 29.75)
spring_7 <- c(32.90, 32.50, 32.80)
spring_8 <- c(36.80, 36.45, 33.15)
```

4. Build a data matrix that has the springs as rows and the columns as scientists.  

```r
temperatures <- c(spring_1, spring_2, spring_3, spring_4, spring_5, spring_6, spring_7, spring_8)
temperatures 
```

```
##  [1] 36.25 35.40 35.30 35.15 35.35 33.35 30.70 29.65 29.20 39.70 40.05 38.65
## [13] 31.85 31.40 29.30 30.20 30.65 29.75 32.90 32.50 32.80 36.80 36.45 33.15
```

```r
springs_matrix <- matrix(temperatures, nrow = 8, byrow= T)
springs_matrix
```

```
##       [,1]  [,2]  [,3]
## [1,] 36.25 35.40 35.30
## [2,] 35.15 35.35 33.35
## [3,] 30.70 29.65 29.20
## [4,] 39.70 40.05 38.65
## [5,] 31.85 31.40 29.30
## [6,] 30.20 30.65 29.75
## [7,] 32.90 32.50 32.80
## [8,] 36.80 36.45 33.15
```


```r
region <- c("Jill", "Steve", "Susan")
region
```

```
## [1] "Jill"  "Steve" "Susan"
```


```r
titles <- c("spring_1", "spring_2", "spring_3", "spring_4", "spring_5", "spring_6", "spring_7", "spring_8")
titles
```

```
## [1] "spring_1" "spring_2" "spring_3" "spring_4" "spring_5" "spring_6" "spring_7"
## [8] "spring_8"
```

Name the columns using `colnames()` with the vector region.

```r
colnames(springs_matrix) <- region
```

Name the rows using `rownames()` with the vector titles.

```r
rownames(springs_matrix) <- titles
```


```r
springs_matrix
```

```
##           Jill Steve Susan
## spring_1 36.25 35.40 35.30
## spring_2 35.15 35.35 33.35
## spring_3 30.70 29.65 29.20
## spring_4 39.70 40.05 38.65
## spring_5 31.85 31.40 29.30
## spring_6 30.20 30.65 29.75
## spring_7 32.90 32.50 32.80
## spring_8 36.80 36.45 33.15
```

5. The names of the springs are 1.Bluebell Spring, 2.Opal Spring, 3.Riverside Spring, 4.Too Hot Spring, 5.Mystery Spring, 6.Emerald Spring, 7.Black Spring, 8.Pearl Spring. Name the rows and columns in the data matrix. Start by making two new vectors with the names, then use `colnames()` and `rownames()` to name the columns and rows.

```r
Bluebell_Spring <- c(spring_1)
Opal_Spring <- (spring_2)
Riverside_Spring <- (spring_3)
Too_Hot_Spring <- (spring_4)
Mystery_Spring <- (spring_5)
Emerald_Spring <- (spring_6)
Black_Spring <- (spring_7)
Pearl_Spring <- (spring_8)
```


```r
titles <- c("Bluebell_Spring", "Opal_Spring", "Riverside_Spring", "Too_Hot_Spring", "Mystery_Spring", "Emerald_Spring", "Black_Spring", "Pearl_Spring")
titles
```

```
## [1] "Bluebell_Spring"  "Opal_Spring"      "Riverside_Spring" "Too_Hot_Spring"  
## [5] "Mystery_Spring"   "Emerald_Spring"   "Black_Spring"     "Pearl_Spring"
```

```r
rownames(springs_matrix) <- titles
```


```r
region <- c("Jill", "Steve", "Susan")
region
```

```
## [1] "Jill"  "Steve" "Susan"
```

```r
colnames(springs_matrix) <- region
```

```r
springs_matrix
```

```
##                   Jill Steve Susan
## Bluebell_Spring  36.25 35.40 35.30
## Opal_Spring      35.15 35.35 33.35
## Riverside_Spring 30.70 29.65 29.20
## Too_Hot_Spring   39.70 40.05 38.65
## Mystery_Spring   31.85 31.40 29.30
## Emerald_Spring   30.20 30.65 29.75
## Black_Spring     32.90 32.50 32.80
## Pearl_Spring     36.80 36.45 33.15
```

6. Calculate the mean temperature of all eight springs.

```r
Mean_Temperature <- rowMeans(springs_matrix)
Mean_Temperature
```

```
##  Bluebell_Spring      Opal_Spring Riverside_Spring   Too_Hot_Spring 
##         35.65000         34.61667         29.85000         39.46667 
##   Mystery_Spring   Emerald_Spring     Black_Spring     Pearl_Spring 
##         30.85000         30.20000         32.73333         35.46667
```

7. Add this as a new column in the data matrix.  

```r
all_springs_matrix <- cbind(springs_matrix, Mean_Temperature)
all_springs_matrix
```

```
##                   Jill Steve Susan Mean_Temperature
## Bluebell_Spring  36.25 35.40 35.30         35.65000
## Opal_Spring      35.15 35.35 33.35         34.61667
## Riverside_Spring 30.70 29.65 29.20         29.85000
## Too_Hot_Spring   39.70 40.05 38.65         39.46667
## Mystery_Spring   31.85 31.40 29.30         30.85000
## Emerald_Spring   30.20 30.65 29.75         30.20000
## Black_Spring     32.90 32.50 32.80         32.73333
## Pearl_Spring     36.80 36.45 33.15         35.46667
```
8. Show Susan's value for Opal Spring only.

```r
springs_matrix[2,3]
```

```
## [1] 33.35
```
9. Calculate the mean for Jill's column only.  

```r
Jill <- springs_matrix[ ,1]
mean(Jill)
```

```
## [1] 34.19375
```
10. Use the data matrix to perform one calculation or operation of your interest.
Here's how you find mean of each scientist's data:

```r
Scientist_Data_Means <- colMeans(springs_matrix)
Scientist_Data_Means
```

```
##     Jill    Steve    Susan 
## 34.19375 33.93125 32.68750
```
## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences. 
