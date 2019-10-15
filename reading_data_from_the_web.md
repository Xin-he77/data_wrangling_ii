reading\_data\_from\_the\_web
================
Xin He
10/10/2019

## get data

read in the NSDUH
data

``` r
url = "http://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm"

drug_use_xml = read_html(url) 

(drug_use_xml %>% html_nodes(css = "table")) %>% 
  .[[1]]
```

    ## {html_node}
    ## <table class="rti" border="1" cellspacing="0" cellpadding="1" width="100%" summary="This is a table containing 16 columns.">
    ## [1] <caption>Table 1 – <span class="boldital">Marijuana Use in the Past  ...
    ## [2] <thead><tr>\n<th class="left" scope="col">State</th>\r\n<th scope="c ...
    ## [3] <tfoot><tr>\n<td scope="row" colspan="16">NOTE: State and census reg ...
    ## [4] <tbody>\n<tr>\n<th scope="row">Total U.S.</th>\r\n<td width="6%" cla ...

``` r
(drug_use_xml %>% html_nodes(css = "table")) %>% class
```

    ## [1] "xml_nodeset"

``` r
table_lsit = (drug_use_xml %>% html_nodes(css = "table"))

tabl_marj = 
  table_lsit[[1]] %>% 
  html_table() %>% 
  slice(-1) %>% 
  as_tibble()
```

## Harry Portor

``` r
hpsaga_html = 
  read_html("https://www.imdb.com/list/ls000630791/")
```

``` r
title_vec = 
  hpsaga_html %>%
  html_nodes(".lister-item-header a") %>%
  html_text()

gross_rev_vec = 
  hpsaga_html %>%
  html_nodes(".text-small:nth-child(7) span:nth-child(5)") %>%
  html_text()

runtime_vec = 
  hpsaga_html %>%
  html_nodes(".runtime") %>%
  html_text()

hpsaga_df = 
  tibble(
    title = title_vec,
    rev = gross_rev_vec,
    runtime = runtime_vec)
```

## Amazon page

``` r
url = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber=1"

dynamite_html = read_html(url)

review_titles = 
  dynamite_html %>%
  html_nodes(".a-text-bold span") %>%
  html_text()

review_stars = 
  dynamite_html %>%
  html_nodes("#cm_cr-review_list .review-rating") %>%
  html_text()

review_text = 
  dynamite_html %>%
  html_nodes(".review-text-content span") %>%
  html_text()

reviews = tibble(
  title = review_titles,
  stars = review_stars,
  text = review_text
)
```

## API

``` r
nyc_water = 
  GET("https://data.cityofnewyork.us/resource/waf7-5gvc.csv") %>% 
  content("parsed")
```

    ## Parsed with column specification:
    ## cols(
    ##   year = col_double(),
    ##   new_york_city_population = col_double(),
    ##   nyc_consumption_million_gallons_per_day = col_double(),
    ##   per_capita_gallons_per_person_per_day = col_double()
    ## )

``` r
nyc_water = 
  GET("https://data.cityofnewyork.us/resource/waf7-5gvc.json") %>% 
  content("text") %>%
  jsonlite::fromJSON() %>%
  as_tibble()
```

``` r
brfss_smart2010 = 
  GET("https://data.cdc.gov/api/views/acme-vg9e/rows.csv?accessType=DOWNLOAD") %>% 
  content("parsed")
```

    ## Parsed with column specification:
    ## cols(
    ##   .default = col_character(),
    ##   Year = col_double(),
    ##   Sample_Size = col_double(),
    ##   Data_value = col_double(),
    ##   Confidence_limit_Low = col_double(),
    ##   Confidence_limit_High = col_double(),
    ##   Display_order = col_double(),
    ##   LocationID = col_logical()
    ## )

    ## See spec(...) for full column specifications.

## Pokemon

``` r
poke = 
  GET("http://pokeapi.co/api/v2/pokemon/1") %>%
  content()

poke$name
```

    ## [1] "bulbasaur"

``` r
poke$abilities
```

    ## [[1]]
    ## [[1]]$ability
    ## [[1]]$ability$name
    ## [1] "chlorophyll"
    ## 
    ## [[1]]$ability$url
    ## [1] "https://pokeapi.co/api/v2/ability/34/"
    ## 
    ## 
    ## [[1]]$is_hidden
    ## [1] TRUE
    ## 
    ## [[1]]$slot
    ## [1] 3
    ## 
    ## 
    ## [[2]]
    ## [[2]]$ability
    ## [[2]]$ability$name
    ## [1] "overgrow"
    ## 
    ## [[2]]$ability$url
    ## [1] "https://pokeapi.co/api/v2/ability/65/"
    ## 
    ## 
    ## [[2]]$is_hidden
    ## [1] FALSE
    ## 
    ## [[2]]$slot
    ## [1] 1
