read\_data
================

## Scrape a table

I want the first table from [this
page](http://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm)

read in the html

``` r
url = "http://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm"

drug_use_html = read_html(url)
```

extract the table(s); focus on first one

``` r
tabl_marj =
  drug_use_html %>%
  html_nodes(css = "table") %>%
  first() %>% #chooses first element- first table in this instance
  html_table() %>%
  slice(-1) %>% #removes first row
  as_tibble()
```

## Star Wars Movie Info

I want the data from [here](https://www.imdb.com/list/ls070150896/)

``` r
url = "https://www.imdb.com/list/ls070150896/"

swm_html = read_html(url)
```

Grab elements that I want- use CSS selector tool to locate element type,
then add into code

``` r
title_vec = 
  swm_html %>%
  html_nodes(css = ".lister-item-header a") %>%
  html_text()

gross_rev_vec = 
  swm_html %>%
  html_nodes(css = ".text-small:nth-child(7) span:nth-child(5)") %>%
  html_text() 

runtime_vec =
  swm_html %>%
  html_nodes(css = ".runtime") %>%
  html_text() 

swm_df =
  tibble(
    title = title_vec,
    gross_rev = gross_rev_vec,
    runtime = runtime_vec
  )
```

## Get some water data

This is coming from an API

``` r
nyc_water =
  GET("https://data.cityofnewyork.us/resource/ia2d-e54m.csv") %>%
  content("parsed")
```

    ## 
    ## -- Column specification --------------------------------------------------------
    ## cols(
    ##   year = col_double(),
    ##   new_york_city_population = col_double(),
    ##   nyc_consumption_million_gallons_per_day = col_double(),
    ##   per_capita_gallons_per_person_per_day = col_double()
    ## )

``` r
#JSON version

#nyc_water =
  #GET("https://data.cityofnewyork.us/resource/ia2d-e54m.json") %>%
  #content() %>%
  #jsonlite::fromJSON() %>%
  #as_tibble()
```

## BRFSS

same process, different data- alter request using query to get
additional rows

``` r
brfss_2010 = 
  GET("https://chronicdata.cdc.gov/resource/acme-vg9e.csv",
      query = list("$limit" = 5000)) %>% #$limit used in api to change amount of rows returned
  content("parsed")
```

    ## 
    ## -- Column specification --------------------------------------------------------
    ## cols(
    ##   .default = col_character(),
    ##   year = col_double(),
    ##   sample_size = col_double(),
    ##   data_value = col_double(),
    ##   confidence_limit_low = col_double(),
    ##   confidence_limit_high = col_double(),
    ##   display_order = col_double(),
    ##   locationid = col_logical()
    ## )
    ## i Use `spec()` for the full column specifications.
