---
title: "Subsetting"
author: "JLB"
date: "2023-08-12"
output: html_document
---



# Subsetting Data in R

- Subsequent data processing setps (statistical analysis, graph generation) works best if you select the data that you are interested in. Splitting up large data sets into multiple subsetted dataframes has many functions in R programming. 

# Get data

```r
library(tidyverse) 
data <- read_csv("EB_Rats_Nicotine_Sensitization.csv")
head(data)
```

```
## # A tibble: 6 × 9
##      ID Sex    Status PREhorm CHALhorm   Hab IND_1 IND_2  CHAL
##   <dbl> <chr>  <chr>    <dbl>    <dbl> <dbl> <dbl> <dbl> <dbl>
## 1     1 Female OVX          1        1 16625 15730 17834 18070
## 2     2 Female OVX          1        1 17097 20482 26562 28665
## 3     3 Female OVX          0        1 17909 23757 17523 22136
## 4     4 Female OVX          0        1 14199 11810 13086 11609
## 5     5 Female OVX          0        0 21715 23184 27482 26809
## 6     6 Female OVX          0        0 14733 19223 23280 23845
```

# Select some of the columns

### Method #1: by position 

- In R language, square brackets [ ] mean "where". Inside of the square brackets, there can be two sets of numbers separated by a comma. Values that come before the comma refer to the ROWS and values that come after the comma refer to the COLUMNS. Blank space means "take all". 

- Square brackets can be used to select a "subset" of a larger dataframe. 


```r
# Select the first 6 columns 
data[ ,1:6] 
```

```
## # A tibble: 47 × 6
##       ID Sex    Status PREhorm CHALhorm   Hab
##    <dbl> <chr>  <chr>    <dbl>    <dbl> <dbl>
##  1     1 Female OVX          1        1 16625
##  2     2 Female OVX          1        1 17097
##  3     3 Female OVX          0        1 17909
##  4     4 Female OVX          0        1 14199
##  5     5 Female OVX          0        0 21715
##  6     6 Female OVX          0        0 14733
##  7     7 Female OVX          1        0 17261
##  8     8 Female OVX          1        0 18035
##  9     9 Female OVX          0        1 15950
## 10    10 Female OVX          0        1 17699
## # ℹ 37 more rows
```

```r
# You can also assign the subsetted data frame onto a new variable 
# It is generally better to create new variables than to overwrite. 
data_2 <- data[ ,1:6] 
head(data_2) 
```

```
## # A tibble: 6 × 6
##      ID Sex    Status PREhorm CHALhorm   Hab
##   <dbl> <chr>  <chr>    <dbl>    <dbl> <dbl>
## 1     1 Female OVX          1        1 16625
## 2     2 Female OVX          1        1 17097
## 3     3 Female OVX          0        1 17909
## 4     4 Female OVX          0        1 14199
## 5     5 Female OVX          0        0 21715
## 6     6 Female OVX          0        0 14733
```

```r
# Select the first 6 rows 
data[1:6, ]
```

```
## # A tibble: 6 × 9
##      ID Sex    Status PREhorm CHALhorm   Hab IND_1 IND_2  CHAL
##   <dbl> <chr>  <chr>    <dbl>    <dbl> <dbl> <dbl> <dbl> <dbl>
## 1     1 Female OVX          1        1 16625 15730 17834 18070
## 2     2 Female OVX          1        1 17097 20482 26562 28665
## 3     3 Female OVX          0        1 17909 23757 17523 22136
## 4     4 Female OVX          0        1 14199 11810 13086 11609
## 5     5 Female OVX          0        0 21715 23184 27482 26809
## 6     6 Female OVX          0        0 14733 19223 23280 23845
```

```r
# Select the first 6 rows of the first 6 columns 
data[1:6,1:6]
```

```
## # A tibble: 6 × 6
##      ID Sex    Status PREhorm CHALhorm   Hab
##   <dbl> <chr>  <chr>    <dbl>    <dbl> <dbl>
## 1     1 Female OVX          1        1 16625
## 2     2 Female OVX          1        1 17097
## 3     3 Female OVX          0        1 17909
## 4     4 Female OVX          0        1 14199
## 5     5 Female OVX          0        0 21715
## 6     6 Female OVX          0        0 14733
```

### Method #2: by content

- Especially when working with larger datasets, you may want to subset based on cell values rather than position within the dataframe. Selecting based on content is a more robust approach because it does not require manual accuracy checks. 


```r
# Select all columns for the rows that have the value "OIL" in the column PREhorm:
data[data$PREhorm == 1, ]
```

```
## # A tibble: 24 × 9
##       ID Sex    Status PREhorm CHALhorm   Hab IND_1 IND_2  CHAL
##    <dbl> <chr>  <chr>    <dbl>    <dbl> <dbl> <dbl> <dbl> <dbl>
##  1     1 Female OVX          1        1 16625 15730 17834 18070
##  2     2 Female OVX          1        1 17097 20482 26562 28665
##  3     7 Female OVX          1        0 17261 28127 29067 23594
##  4     8 Female OVX          1        0 18035 23292 26436 28371
##  5    11 Female OVX          1        0 13911 14925 19742 22440
##  6    12 Female OVX          1        0 15375 18688 21601 27472
##  7    13 Female OVX          1        0 19476 13378 16030 20599
##  8    14 Female OVX          1        0 15163 18267 22373 27165
##  9    15 Female OVX          1        1 14391 17036 17698 20912
## 10    16 Female OVX          1        1 14596 13992 16237 22711
## # ℹ 14 more rows
```


```r
# Can also include multiple criteria: 
data[data$PREhorm == 1 & data$CHALhorm == 1, ]
```

```
## # A tibble: 12 × 9
##       ID Sex    Status PREhorm CHALhorm   Hab IND_1 IND_2  CHAL
##    <dbl> <chr>  <chr>    <dbl>    <dbl> <dbl> <dbl> <dbl> <dbl>
##  1     1 Female OVX          1        1 16625 15730 17834 18070
##  2     2 Female OVX          1        1 17097 20482 26562 28665
##  3    15 Female OVX          1        1 14391 17036 17698 20912
##  4    16 Female OVX          1        1 14596 13992 16237 22711
##  5    17 Female OVX          1        1 17159 17765 22125 27588
##  6    18 Female OVX          1        1 15261 10931 16726 20820
##  7    27 Female OVX          1        1 18089 25959 26011 31956
##  8    28 Female OVX          1        1 14072 16945 18627 21871
##  9    31 Female OVX          1        1 18703 17198 24054 26078
## 10    32 Female OVX          1        1 14472 15254 15859 18728
## 11    43 Female OVX          1        1 14585 15684 11912 22958
## 12    44 Female OVX          1        1 19446 19920 22839 30769
```

### Method #3: dplyr "select"

- You can also use the "select" function inside of dplyr code blocks. This is generally the most elegant solution to choosing a subset of your dataframe to work with.


```r
data %>%
  select(c("ID","PREhorm","Hab"))
```

```
## # A tibble: 47 × 3
##       ID PREhorm   Hab
##    <dbl>   <dbl> <dbl>
##  1     1       1 16625
##  2     2       1 17097
##  3     3       0 17909
##  4     4       0 14199
##  5     5       0 21715
##  6     6       0 14733
##  7     7       1 17261
##  8     8       1 18035
##  9     9       0 15950
## 10    10       0 17699
## # ℹ 37 more rows
```





