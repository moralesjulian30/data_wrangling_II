01_reading_from_web
================
Julian Morales
2026-10-20

``` r
library(rvest)
library(httr)
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.2.1     ✔ readr     2.2.0
    ## ✔ forcats   1.0.1     ✔ stringr   1.6.0
    ## ✔ ggplot2   4.0.3     ✔ tibble    3.3.1
    ## ✔ lubridate 1.9.5     ✔ tidyr     1.3.2
    ## ✔ purrr     1.2.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter()         masks stats::filter()
    ## ✖ readr::guess_encoding() masks rvest::guess_encoding()
    ## ✖ dplyr::lag()            masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

\##Import NSDUH

``` r
url = "https://www.samhsa.gov/data/sites/default/files/NSDUHsaeShortTermCHG2015/NSDUHsaeShortTermCHG2015.htm"

drug_use_html = 
  read_html(url)
```

pull the tables out

``` r
marj_df = 
  drug_use_html |> 
  html_table() |> #gets all tables in html
  first() |> #takes first table
  slice(-1) #removes rows not wanted
```

\##Import some star wars data

pull in the html

``` r
swm_html = 
  read_html_live("https://www.imdb.com/list/ls070150896/")
```

next up …. selector gadget

``` r
swm_title_vec =
  swm_html|> 
  html_elements(".ipc-title-link-wrapper .ipc-title__text") |> 
  html_text()

swm_metascore_vec =
  swm_html |> 
  html_elements(".metacritic-score-box") |> 
  html_text()

swm_runtime_vec = 
  swm_html |> 
  html_elements(".ipc-inline-list__item:nth-child(2)") |> 
  html_text()

swm_df = 
  tibble(
    title = swm_title_vec, 
    score = swm_metascore_vec,
    runtime = swm_runtime_vec
  )
```
