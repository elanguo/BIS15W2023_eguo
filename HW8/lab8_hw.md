---
title: "Lab 8 Homework"
author: "Elan Guo"
date: "2023-02-09"
output:
  html_document:
    theme: spacelab
    toc: no
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the libraries

```r
library(tidyverse)
library(janitor)
```

## Install `here`
The package `here` is a nice option for keeping directories clear when loading files. I will demonstrate below and let you decide if it is something you want to use.  

## Data
For this homework, we will use a data set compiled by the Office of Environment and Heritage in New South Whales, Australia. It contains the enterococci counts in water samples obtained from Sydney beaches as part of the Beachwatch Water Quality Program. Enterococci are bacteria common in the intestines of mammals; they are rarely present in clean water. So, enterococci values are a measurement of pollution. `cfu` stands for colony forming units and measures the number of viable bacteria in a sample [cfu](https://en.wikipedia.org/wiki/Colony-forming_unit).   

This homework loosely follows the tutorial of [R Ladies Sydney](https://rladiessydney.org/). If you get stuck, check it out!  

1. Start by loading the data `sydneybeaches`. Do some exploratory analysis to get an idea of the data structure.

```r
library(here)
```

```
## here() starts at /Users/elanguo/Desktop/BIS15W2023_eguo
```

```r
here()
```

```
## [1] "/Users/elanguo/Desktop/BIS15W2023_eguo"
```

```r
sydneybeaches <- read_csv(here("lab8", "data", "sydneybeaches.csv"))
```

```
## Rows: 3690 Columns: 8
```

```
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (4): Region, Council, Site, Date
## dbl (4): BeachId, Longitude, Latitude, Enterococci (cfu/100ml)
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
glimpse(sydneybeaches)
```

```
## Rows: 3,690
## Columns: 8
## $ BeachId                   <dbl> 25, 25, 25, 25, 25, 25, 25, 25, 25, 25, 25, …
## $ Region                    <chr> "Sydney City Ocean Beaches", "Sydney City Oc…
## $ Council                   <chr> "Randwick Council", "Randwick Council", "Ran…
## $ Site                      <chr> "Clovelly Beach", "Clovelly Beach", "Clovell…
## $ Longitude                 <dbl> 151.2675, 151.2675, 151.2675, 151.2675, 151.…
## $ Latitude                  <dbl> -33.91449, -33.91449, -33.91449, -33.91449, …
## $ Date                      <chr> "02/01/2013", "06/01/2013", "12/01/2013", "1…
## $ `Enterococci (cfu/100ml)` <dbl> 19, 3, 2, 13, 8, 7, 11, 97, 3, 0, 6, 0, 1, 8…
```

If you want to try `here`, first notice the output when you load the `here` library. It gives you information on the current working directory. You can then use it to easily and intuitively load files.

```r
library(here)
```

The quotes show the folder structure from the root directory.

```r
sydneybeaches <-read_csv(here("lab8", "data", "sydneybeaches.csv")) %>% janitor::clean_names()
```

```
## Rows: 3690 Columns: 8
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (4): Region, Council, Site, Date
## dbl (4): BeachId, Longitude, Latitude, Enterococci (cfu/100ml)
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

2. Are these data "tidy" per the definitions of the tidyverse? How do you know? Are they in wide or long format?

```r
#The data is not tidy. It's in long format.
```

3. We are only interested in the variables site, date, and enterococci_cfu_100ml. Make a new object focused on these variables only. Name the object `sydneybeaches_long`

```r
sydneybeaches_long <- sydneybeaches %>%
  select(site, date, enterococci_cfu_100ml)
sydneybeaches_long
```

```
## # A tibble: 3,690 × 3
##    site           date       enterococci_cfu_100ml
##    <chr>          <chr>                      <dbl>
##  1 Clovelly Beach 02/01/2013                    19
##  2 Clovelly Beach 06/01/2013                     3
##  3 Clovelly Beach 12/01/2013                     2
##  4 Clovelly Beach 18/01/2013                    13
##  5 Clovelly Beach 30/01/2013                     8
##  6 Clovelly Beach 05/02/2013                     7
##  7 Clovelly Beach 11/02/2013                    11
##  8 Clovelly Beach 23/02/2013                    97
##  9 Clovelly Beach 07/03/2013                     3
## 10 Clovelly Beach 25/03/2013                     0
## # … with 3,680 more rows
```

4. Pivot the data such that the dates are column names and each beach only appears once. Name the object `sydneybeaches_wide`

```r
sydneybeaches_wide <- sydneybeaches_long %>%
  pivot_wider(names_from = "date",
              values_from = "enterococci_cfu_100ml")
sydneybeaches_wide
```

```
## # A tibble: 11 × 345
##    site  02/01…¹ 06/01…² 12/01…³ 18/01…⁴ 30/01…⁵ 05/02…⁶ 11/02…⁷ 23/02…⁸ 07/03…⁹
##    <chr>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>
##  1 Clov…      19       3       2      13       8       7      11      97       3
##  2 Coog…      15       4      17      18      22       2     110     630      11
##  3 Gord…      NA      NA      NA      NA      NA      NA      NA      NA      NA
##  4 Litt…       9       3      72       1      44       7     150     330      31
##  5 Mala…       2       4     390      15      13      13     140     390       6
##  6 Maro…       1       1      20       2      11       0       4      60       1
##  7 Sout…       1       0      33       2      13       0      30      92      13
##  8 Sout…      12       2     110      13     100     630      79     570      69
##  9 Bond…       3       1       2       1       6       5     600      67       1
## 10 Bron…       4       2      38       3      25       2      38     780       8
## 11 Tama…       1       0       7      22      23       3      13      90       3
## # … with 335 more variables: `25/03/2013` <dbl>, `02/04/2013` <dbl>,
## #   `12/04/2013` <dbl>, `18/04/2013` <dbl>, `24/04/2013` <dbl>,
## #   `01/05/2013` <dbl>, `20/05/2013` <dbl>, `31/05/2013` <dbl>,
## #   `06/06/2013` <dbl>, `12/06/2013` <dbl>, `24/06/2013` <dbl>,
## #   `06/07/2013` <dbl>, `18/07/2013` <dbl>, `24/07/2013` <dbl>,
## #   `08/08/2013` <dbl>, `22/08/2013` <dbl>, `29/08/2013` <dbl>,
## #   `24/01/2013` <dbl>, `17/02/2013` <dbl>, `01/03/2013` <dbl>, …
```

5. Pivot the data back so that the dates are data and not column names.

```r
sydneybeaches_wide %>%
  pivot_longer(-site,
               names_to = "date",
               values_to = "enterococci_cfu_100ml")
```

```
## # A tibble: 3,784 × 3
##    site           date       enterococci_cfu_100ml
##    <chr>          <chr>                      <dbl>
##  1 Clovelly Beach 02/01/2013                    19
##  2 Clovelly Beach 06/01/2013                     3
##  3 Clovelly Beach 12/01/2013                     2
##  4 Clovelly Beach 18/01/2013                    13
##  5 Clovelly Beach 30/01/2013                     8
##  6 Clovelly Beach 05/02/2013                     7
##  7 Clovelly Beach 11/02/2013                    11
##  8 Clovelly Beach 23/02/2013                    97
##  9 Clovelly Beach 07/03/2013                     3
## 10 Clovelly Beach 25/03/2013                     0
## # … with 3,774 more rows
```

6. We haven't dealt much with dates yet, but separate the date into columns day, month, and year. Do this on the `sydneybeaches_long` data.

```r
sydneybeaches_separate <- sydneybeaches_long %>%
  separate(date, into=c("day", "month", "year"), sep="/")
sydneybeaches_separate
```

```
## # A tibble: 3,690 × 5
##    site           day   month year  enterococci_cfu_100ml
##    <chr>          <chr> <chr> <chr>                 <dbl>
##  1 Clovelly Beach 02    01    2013                     19
##  2 Clovelly Beach 06    01    2013                      3
##  3 Clovelly Beach 12    01    2013                      2
##  4 Clovelly Beach 18    01    2013                     13
##  5 Clovelly Beach 30    01    2013                      8
##  6 Clovelly Beach 05    02    2013                      7
##  7 Clovelly Beach 11    02    2013                     11
##  8 Clovelly Beach 23    02    2013                     97
##  9 Clovelly Beach 07    03    2013                      3
## 10 Clovelly Beach 25    03    2013                      0
## # … with 3,680 more rows
```

7. What is the average `enterococci_cfu_100ml` by year for each beach. Think about which data you will use- long or wide.

```r
sydneybeaches_separate %>%
  pivot_wider(names_from = "year",
              values_from = "enterococci_cfu_100ml")
```

```
## # A tibble: 2,656 × 9
##    site           day   month `2013` `2014` `2015` `2016` `2017` `2018`
##    <chr>          <chr> <chr>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>
##  1 Clovelly Beach 02    01        19     NA     NA     NA     NA     NA
##  2 Clovelly Beach 06    01         3     NA     NA     NA     NA     NA
##  3 Clovelly Beach 12    01         2     NA     NA     NA     NA     NA
##  4 Clovelly Beach 18    01        13     NA     NA     NA     NA     NA
##  5 Clovelly Beach 30    01         8     NA     NA     NA      8     NA
##  6 Clovelly Beach 05    02         7     NA     NA     NA     NA     NA
##  7 Clovelly Beach 11    02        11     NA     NA     NA     NA     NA
##  8 Clovelly Beach 23    02        97     NA     NA     NA     NA     NA
##  9 Clovelly Beach 07    03         3     NA     NA     NA     NA     NA
## 10 Clovelly Beach 25    03         0     NA     11     NA     NA     NA
## # … with 2,646 more rows
```

8. Make the output from question 7 easier to read by pivoting it to wide format.

```r
sydneybeaches_separate %>%
  pivot_wider(-c(month, day),
              names_from = "year",
              values_from = "enterococci_cfu_100ml")
```

```
## Warning: Values from `enterococci_cfu_100ml` are not uniquely identified; output will contain list-cols.
## * Use `values_fn = list` to suppress this warning.
## * Use `values_fn = {summary_fun}` to summarise duplicates.
## * Use the following dplyr code to identify duplicates.
##   {data} %>%
##     dplyr::group_by(site, year) %>%
##     dplyr::summarise(n = dplyr::n(), .groups = "drop") %>%
##     dplyr::filter(n > 1L)
```

```
## # A tibble: 11 × 7
##    site                    `2013`     `2014`     `2015`     `2016` `2017` `2018`
##    <chr>                   <list>     <list>     <list>     <list> <list> <list>
##  1 Clovelly Beach          <dbl [58]> <dbl [53]> <dbl [57]> <dbl>  <dbl>  <dbl> 
##  2 Coogee Beach            <dbl [58]> <dbl [52]> <dbl [57]> <dbl>  <dbl>  <dbl> 
##  3 Gordons Bay (East)      <dbl [44]> <dbl [53]> <dbl [57]> <dbl>  <dbl>  <dbl> 
##  4 Little Bay Beach        <dbl [58]> <dbl [53]> <dbl [57]> <dbl>  <dbl>  <dbl> 
##  5 Malabar Beach           <dbl [58]> <dbl [53]> <dbl [57]> <dbl>  <dbl>  <dbl> 
##  6 Maroubra Beach          <dbl [58]> <dbl [53]> <dbl [57]> <dbl>  <dbl>  <dbl> 
##  7 South Maroubra Beach    <dbl [58]> <dbl [53]> <dbl [57]> <dbl>  <dbl>  <dbl> 
##  8 South Maroubra Rockpool <dbl [36]> <dbl [53]> <dbl [57]> <dbl>  <dbl>  <dbl> 
##  9 Bondi Beach             <dbl [58]> <dbl [53]> <dbl [57]> <dbl>  <dbl>  <dbl> 
## 10 Bronte Beach            <dbl [58]> <dbl [53]> <dbl [57]> <dbl>  <dbl>  <dbl> 
## 11 Tamarama Beach          <dbl [58]> <dbl [53]> <dbl [57]> <dbl>  <dbl>  <dbl>
```

9. What was the most polluted beach in 2018?

```r
#Clovelly Beach
```

10. Please complete the class project survey at: [BIS 15L Group Project](https://forms.gle/H2j69Z3ZtbLH3efW6)

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
