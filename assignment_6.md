assignment 6
================

Table of contents
=================

-   [Task 1: Character Data](#Task%201%20Character%20Data)
    -   [Exrercise 14.2.5](#Exrercise%2014.2.5)

Writing Functions
=================

``` r
suppressPackageStartupMessages(library(gapminder))
suppressPackageStartupMessages(library(tidyverse))
suppressPackageStartupMessages(library(ggplot2)) 
suppressPackageStartupMessages(library(dplyr))
suppressPackageStartupMessages(library(MASS))
suppressPackageStartupMessages(library(singer))
suppressPackageStartupMessages(library(ggmap))
suppressPackageStartupMessages(library(singer)) 
```

\# Task 1 Character Data
========================

In this task, I have read and worked the exercises in the **Strings chapter** or R for Data Science.

First we will load the library needed for this exercise

``` r
suppressPackageStartupMessages(library(tidyverse))
library(stringr)
```

\# Exrercise 14.2.5
-------------------

1.  In code that doesn’t use stringr, you’ll often see `paste()` and `paste0()`. What’s the difference between the two functions? What stringr function are they equivalent to? How do the functions differ in their handling of NA?

``` r
?paste()
```

    ## starting httpd help server ... done

``` r
?paste0()
```

Task 2
------

**1** First we extract the data for Canada to work on the code

``` r
chosen_country <- "Canada"
(chosen_data <- gapminder %>% 
  filter(country == chosen_country))
```

    ## # A tibble: 12 x 6
    ##    country continent  year lifeExp      pop gdpPercap
    ##    <fct>   <fct>     <int>   <dbl>    <int>     <dbl>
    ##  1 Canada  Americas   1952    68.8 14785584    11367.
    ##  2 Canada  Americas   1957    70.0 17010154    12490.
    ##  3 Canada  Americas   1962    71.3 18985849    13462.
    ##  4 Canada  Americas   1967    72.1 20819767    16077.
    ##  5 Canada  Americas   1972    72.9 22284500    18971.
    ##  6 Canada  Americas   1977    74.2 23796400    22091.
    ##  7 Canada  Americas   1982    75.8 25201900    22899.
    ##  8 Canada  Americas   1987    76.9 26549700    26627.
    ##  9 Canada  Americas   1992    78.0 28523502    26343.
    ## 10 Canada  Americas   1997    78.6 30305843    28955.
    ## 11 Canada  Americas   2002    79.8 31902268    33329.
    ## 12 Canada  Americas   2007    80.7 33390141    36319.

Then we plot the graph of gdp per capita against population and we can use a polynomial function instead of a linear function to better fit the data.

``` r
p <- ggplot(chosen_data, aes(x = pop, y = gdpPercap))
p + geom_point() + geom_smooth(method = 'lm', se = FALSE)
```

![](assignment_6_files/figure-markdown_github/unnamed-chunk-5-1.png)

We fit the data with a cubic function, and the coefficient of x with different degree is shown:

``` r
cub_fit <- rlm(gdpPercap ~ year+I(year^2)+I(year^3),chosen_data)
coef(cub_fit)
```

    ##   (Intercept)          year     I(year^2)     I(year^3) 
    ## -1.888527e+08  2.895614e+05 -1.481768e+02  2.530862e-02

we can then use then plot the cubic function

``` r
 curve(predict(cub_fit,data.frame(year=x)),col='blue',lwd=2) 
```

![](assignment_6_files/figure-markdown_github/unnamed-chunk-7-1.png)

Then we now sum up the above codes to become a function and try out the data above: with the below function, by inputing the corresponding country name, we could get the cubic regression for the gdp per capita against population for this country.

``` r
cubic_curve_fit  <-  function (chosen_country){
  chosen_data <- gapminder %>% 
  filter(country == chosen_country)
  
  fit_curve <- rlm(gdpPercap ~ year+I(year^2)+I(year^3), chosen_data)
  setNames(coef(fit_curve),c("intercept","x","x^2","x^3"))
}

cubic_curve_fit("Canada")
```

    ##     intercept             x           x^2           x^3 
    ## -1.888527e+08  2.895614e+05 -1.481768e+02  2.530862e-02

Again we can try to use this on other countries:

``` r
cubic_curve_fit("France")
```

    ##     intercept             x           x^2           x^3 
    ##  2.489145e+08 -3.793022e+05  1.924422e+02 -3.250638e-02

``` r
cubic_curve_fit("Afghanistan")
```

    ##     intercept             x           x^2           x^3 
    ## -1.053021e+08  1.597388e+05 -8.076768e+01  1.361201e-02

``` r
cubic_curve_fit("Japan")
```

    ##     intercept             x           x^2           x^3 
    ##  1.680453e+09 -2.550644e+06  1.290149e+03 -2.174665e-01

Task 4 Work With the `singer` data
----------------------------------

**Use purrr to map latitude and longitude into human readable information on the band’s origin places.**

First, we use head() function to visulise the singer\_location dataframe. We realised that there are quite a lot of `NA` appeared in the entry. We need to get rid of these before tidy up the data.

``` r
head(singer_locations)
```

    ## # A tibble: 6 x 14
    ##   track_id title song_id release artist_id artist_name  year duration
    ##   <chr>    <chr> <chr>   <chr>   <chr>     <chr>       <int>    <dbl>
    ## 1 TRWICRA~ The ~ SOSURT~ Even I~ ARACDPV1~ Motion Cit~  2007     170.
    ## 2 TRXJANY~ Lone~ SODESQ~ The Du~ ARYBUAO1~ Gene Chand~  2004     107.
    ## 3 TRIKPCA~ Here~ SOQUYQ~ Improm~ AR4111G1~ Paul Horn    1998     528.
    ## 4 TRYEATD~ Rego~ SOEZGR~ Still ~ ARQDZP31~ Ronnie Ear~  1995     695.
    ## 5 TRBYYXH~ Games SOPIOC~ Afro-H~ AR75GYU1~ Dorothy As~  1968     237.
    ## 6 TRKFFKR~ More~ SOHQSP~ Six Ya~ ARCENE01~ Barleyjuice  2006     193.
    ## # ... with 6 more variables: artist_hotttnesss <dbl>,
    ## #   artist_familiarity <dbl>, latitude <dbl>, longitude <dbl>, name <chr>,
    ## #   city <chr>

``` r
tidy_data <- singer_locations %>% 
  filter(!is.na(latitude)|!is.na(longitude)|!is.na(city)) %>% 
  dplyr::select(name,city,longitude,latitude)
# while we load the library MASS together with the dplyr, we need to use dplyr::select to call this function
head(tidy_data) %>% 
  knitr::kable()
```

| name                        | city            |    longitude|                                                                                                                                                                 latitude|
|:----------------------------|:----------------|------------:|------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| Gene Chandler               | Chicago, IL     |    -87.63241|                                                                                                                                                                 41.88415|
| Paul Horn                   | New York, NY    |    -74.00712|                                                                                                                                                                 40.71455|
| Dorothy Ashby               | Detroit, MI     |    -83.04792|                                                                                                                                                                 42.33168|
| Barleyjuice                 | Pennsylvania    |    -77.60454|                                                                                                                                                                 40.99471|
| Madlib                      | Oxnard, CA      |   -119.18044|                                                                                                                                                                 34.20034|
| Seeed feat. Elephant Man    | Bonn            |      7.10169|                                                                                                                                                                 50.73230|
| next we are going to map th | e longitude and |  latitude to|  the band's origin place, and the function used in the mapping is `map2()` and the `revgeocode()` from `ggmap` is used to reverse a geo code location using Google Maps.|

``` r
# location <- map2_chr(tidy_data$longitude[30:100],tidy_data$latitude[30:100], ~revgeocode(as.numeric(c(.x,.y)), output ="more" , source = "google"))
```
