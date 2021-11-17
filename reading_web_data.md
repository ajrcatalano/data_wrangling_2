Reading Data from the Web
================
AJ Catalano
11/17/2021

``` r
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──

    ## ✓ ggplot2 3.3.5     ✓ purrr   0.3.4
    ## ✓ tibble  3.1.5     ✓ dplyr   1.0.7
    ## ✓ tidyr   1.1.4     ✓ stringr 1.4.0
    ## ✓ readr   2.0.2     ✓ forcats 0.5.1

    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
library(rvest)
```

    ## 
    ## Attaching package: 'rvest'

    ## The following object is masked from 'package:readr':
    ## 
    ##     guess_encoding

``` r
library(httr)
```

## Scraping Table from Webpage

Acquiring data from Table 1 on [this
page](http://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm)

``` r
marijuana_url = "http://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm"

# reading in drug use html

drug_use_html = read_html(marijuana_url)

# extracting table(s)
# look at `first` options to select different rows

marijuana_use = 
  drug_use_html %>% 
  html_nodes(css = "table") %>%
  first() %>% 
  html_table() %>%
  slice(-1) %>% 
  as_tibble()
```

## Scraping from something other than a table

Star Wars films data from [IMDb](https://www.imdb.com/list/ls070150896/)

``` r
sw_url = "https://www.imdb.com/list/ls070150896/"

star_wars_html = read_html(sw_url)

title_vec = 
  star_wars_html %>% 
  html_nodes(css = ".lister-item-header a") %>% 
  html_text()

gross_rev_vec = 
  star_wars_html %>% 
  html_nodes(css = ".text-small:nth-child(7) span:nth-child(5)") %>% 
  html_text()

runtime_vec = 
  star_wars_html %>% 
  html_nodes(css = ".runtime") %>% 
  html_text()

starwars_df = 
  tibble(
    title = title_vec,
    gross_rev = gross_rev_vec,
    runtime = runtime_vec)
```
