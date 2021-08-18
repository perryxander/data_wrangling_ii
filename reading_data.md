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
