Midterm
================
Kaila An
2022-10-19

``` r
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.2 ──
    ## ✔ ggplot2 3.3.6     ✔ purrr   0.3.4
    ## ✔ tibble  3.1.8     ✔ dplyr   1.0.9
    ## ✔ tidyr   1.2.0     ✔ stringr 1.4.1
    ## ✔ readr   2.1.2     ✔ forcats 0.5.2
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
library(data.table)
```

    ## 
    ## Attaching package: 'data.table'
    ## 
    ## The following objects are masked from 'package:dplyr':
    ## 
    ##     between, first, last
    ## 
    ## The following object is masked from 'package:purrr':
    ## 
    ##     transpose

``` r
library(dplyr)
library(dtplyr)
library(ggplot2)
```

## Introduction and Formulated Question

For this project, I chose to focus on Los Angeles City Data,
specifically, ‘Crime Data in LA from 2020 to present’ and ‘Crime Data in
LA from 2010 to 2019’. These particular data sets document incidents of
crime, transcribed from crime reports. This data has been provided by
the Los Angeles Police Department. Based on these data sets, the
question I would like to address is:

How has the prevalence of certain types of crimes changed in LA county
since the COVID-19 pandemic?

## Methods

# Reading in the data

``` r
lacrime202022 <- data.table::fread("crimedata_2020topresent.csv")
lacrime201019 <- data.table::fread("crime data_2010to2019.csv")
```

## Preliminary Results

## Conclusion
