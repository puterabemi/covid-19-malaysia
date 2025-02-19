PKD Kinta COVID19 Cases for May 2021
================

# Introduction

This page created to make simple visualization for COVID-19 cases
specifically for PKD Kinta.

Data for COVID-19 cases for Malaysia were forked from Dr Wan Ariffin
github, while the count for COVID-19 cases for PKD Kinta were manually
build from daily reported cases in MOH website.

The original reason for this visualization is for prepare manuscript for
research articles on EMCO in PKD Kinta.

# Library and Data

``` r
library(pacman)
p_load(readr, tidyverse, lubridate,zoo)

covid_19_my <- read_csv("covid-19_my.csv")
covid_19_my_state <- read_csv("covid-19_my_state.csv")
covid_19_kinta <- read_csv("covid-19_kinta.csv")
covid_19_kinta_epidweek <- read_csv("covid-19_kinta_epidweekcases.csv")
covid_19_kinta_mukim_epidweek <- read_csv("covid-19_kinta_mukim_epidweekcases.csv") |>
  pivot_longer(!ME, names_to = "MUKIM", values_to = "IR")
```

# Malaysia & Perak Overview

Malaysia daily new cases, from January 2021, until June 2021

``` r
covid_19_my |> 
  filter(date > "2021-01-01" & date < "2021-07-01") |> 
  ggplot(aes(x=date, y = new_cases)) + 
  geom_line()
```

![](KintaCasesVisualization_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

``` r
covid_19_my_state |> 
  filter(date > "2021-01-01" & date < "2021-07-01") |> 
  ggplot(aes(x=date, y = new_cases, colour = state)) + 
  geom_line()
```

![](KintaCasesVisualization_files/figure-gfm/unnamed-chunk-2-2.png)<!-- -->

Perak

``` r
covid_19_my_state |> 
  filter(state == "PERAK") |> 
  filter(date > "2021-01-01" & date < "2021-07-01") |> 
  ggplot(aes(x=date, y = new_cases)) + 
  geom_line()
```

![](KintaCasesVisualization_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

``` r
covid_19_my_state |> 
  filter(state == "PERAK") |> 
  filter(date > "2021-01-01" & date < "2021-07-01") |> 
  mutate(newcase7ave = rollmean(new_cases,7, fill = NA)) |> 
  ggplot(aes(x=date, y = newcase7ave)) + 
  geom_line() +
  scale_x_date(date_breaks = "1 months", date_labels = "%F")
```

    ## Warning: Removed 6 row(s) containing missing values (geom_path).

![](KintaCasesVisualization_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

Kinta

``` r
covid_19_kinta |> 
  ggplot(aes(x=date, y = new_cases)) + 
  geom_line()
```

![](KintaCasesVisualization_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

``` r
covid_19_kinta |> 
  mutate(newcase7ave = rollmean(new_cases,7, fill = NA)) |> 
  ggplot(aes(x=date, y = newcase7ave)) + 
  geom_line() +
  scale_x_date(date_breaks = "1 months", date_labels = "%F")
```

    ## Warning: Removed 6 row(s) containing missing values (geom_path).

![](KintaCasesVisualization_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

# PKD Kinta COVID-19 Cases in 2021 By Epid Week

Epid Week 1 (ME1) = 3rd - 9th Jan 2021 ME25 = 20th - 26th June 2021

``` r
covid_19_kinta_epidweek|> 
  ggplot(aes(x=ME, y = new_cases)) + 
  geom_bar(stat="identity") +
  scale_x_continuous(breaks = seq(1,25,1))
```

![](KintaCasesVisualization_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

Incidence Rate per 100,000 population

Daerah Kinta

``` r
covid_19_kinta_epidweek |> 
  ggplot(aes(x=ME, y=IR)) +
  geom_line() +
  scale_x_continuous(breaks = seq(1,25,1))
```

![](KintaCasesVisualization_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

Mukim in Daerah Kinta

``` r
covid_19_kinta_mukim_epidweek |> 
  ggplot(aes(x=ME, y=IR, color = MUKIM)) +
  geom_line() +
  scale_x_continuous(breaks = seq(1,25,1))
```

![](KintaCasesVisualization_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

``` r
ggplot() +
  geom_line(data=covid_19_kinta_epidweek, aes(ME, IR)) +
  geom_line(data=covid_19_kinta_mukim_epidweek, aes(ME, IR, color = MUKIM)) +
#  scale_y_continuous(limits = c(0,90)) +
  scale_x_continuous(breaks = seq(1,25,1)) 
```

![](KintaCasesVisualization_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->
