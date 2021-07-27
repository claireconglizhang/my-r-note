---
title: "My R Notes for Data management"
author: "Congli (Claire) Zhang"
date: "Starting at: 6/28/2021"
output:
  html_document:
    keep_md: yes
---



This is my notes of R code for data manipulation and management.

Organizing the notes using:

 * header 2, area of use, e.g., visualization, clean and tidy data, modeling, etc.
 * header 3, package::function
 * header 4, whatever important heading for that function


## 1. Importing and exporting data

When you don't have a project created beforehand:

```r
# here::i_am("code/filename.R")
# fwrite(df, "data/df.csv")
```


## 2. Inspecting the dataset

### Base R, table()
create tabular results of categorical variables.
check for one variable:

```r
table(mtcars$cyl) # does not report NA's
```

```
## 
##  4  6  8 
## 11  7 14
```

```r
table(mtcars$cyl, exclude = NULL) # reports NA's
```

```
## 
##  4  6  8 
## 11  7 14
```

```r
table(mtcars$cyl, exclude = "4")
```

```
## 
##  6  8 
##  7 14
```

```r
table(mtcars$gear > 4)
```

```
## 
## FALSE  TRUE 
##    27     5
```

```r
table(mtcars$gear > 4, useNA = "always")
```

```
## 
## FALSE  TRUE  <NA> 
##    27     5     0
```

contingency table for two variables:

```r
table(mtcars$cyl, is.na(mtcars$carb))
```

```
##    
##     FALSE
##   4    11
##   6     7
##   8    14
```

### dplyr::mutate()
recode:

```r
foo <- mtcars %>% 
  mutate(cyl = recode(cyl, "4" = "four", "6" = "six", "8" = "eight"))
```

panel data strategy - you may want to set flag for cases when you have missing values on a time-invariant variable, gender for example, you want to code the missing value from a certain year by using the non-missing value from other years:

```r
foo <- mtcars %>%
  group_by(cyl) %>% 
  arrange(gear) %>% 
  mutate(last_obs_fg = row_number() == n()) %>% 
  mutate(carb_new = ifelse(carb == 4, carb, carb[last_obs_fg == TRUE]))
```


## 3. Cleaning and tidying data



