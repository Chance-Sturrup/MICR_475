Homework 8
================

\#Question 1 Plotting the relationship of price as a function of weight
for each color in order of decreasing slope for the *diamonds* data set.

``` r
library(tidyverse)
library(modelr)
glimpse(diamonds)
```

    ## Rows: 53,940
    ## Columns: 10
    ## $ carat   <dbl> 0.23, 0.21, 0.23, 0.29, 0.31, 0.24, 0.24, 0.26, 0.22, 0.23, 0.…
    ## $ cut     <ord> Ideal, Premium, Good, Premium, Good, Very Good, Very Good, Ver…
    ## $ color   <ord> E, E, E, I, J, J, I, H, E, H, J, J, F, J, E, E, I, J, J, J, I,…
    ## $ clarity <ord> SI2, SI1, VS1, VS2, SI2, VVS2, VVS1, SI1, VS2, VS1, SI1, VS1, …
    ## $ depth   <dbl> 61.5, 59.8, 56.9, 62.4, 63.3, 62.8, 62.3, 61.9, 65.1, 59.4, 64…
    ## $ table   <dbl> 55, 61, 65, 58, 58, 57, 57, 55, 61, 61, 55, 56, 61, 54, 62, 58…
    ## $ price   <int> 326, 326, 327, 334, 335, 336, 336, 337, 337, 338, 339, 340, 34…
    ## $ x       <dbl> 3.95, 3.89, 4.05, 4.20, 4.34, 3.94, 3.95, 4.07, 3.87, 4.00, 4.…
    ## $ y       <dbl> 3.98, 3.84, 4.07, 4.23, 4.35, 3.96, 3.98, 4.11, 3.78, 4.05, 4.…
    ## $ z       <dbl> 2.43, 2.31, 2.31, 2.63, 2.75, 2.48, 2.47, 2.53, 2.49, 2.39, 2.…

``` r
ggplot(data = diamonds) +
  geom_point(aes(x=carat, y=price, color = color))
```

![](hw_8_files/figure-gfm/unnamed-chunk-1-1.png)<!-- -->

``` r
dmd <- diamonds %>%
  group_by(color) %>%
  nest()
```

``` r
color_model <- function(df)
  lm(price~carat, data = df)

dmd <- dmd %>%
  mutate(model = map(data, color_model))

dmd <- dmd %>%
  mutate(predicts = map2(data, model, add_predictions))

predicts <- unnest(dmd, predicts)
```

``` r
ggplot(data = predicts) +
  geom_line(aes(x=carat, y=pred)) +
  geom_point(aes(x=carat, y=price)) +
  facet_wrap(~color)
```

![](hw_8_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

\#Question 2

Nonlinear least squares models.

``` r
library(tidyverse)
glimpse(DNase)
```

    ## Rows: 176
    ## Columns: 3
    ## $ Run     <ord> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 2, 2, 2, 2,…
    ## $ conc    <dbl> 0.04882812, 0.04882812, 0.19531250, 0.19531250, 0.39062500, 0.…
    ## $ density <dbl> 0.017, 0.018, 0.121, 0.124, 0.206, 0.215, 0.377, 0.374, 0.614,…

``` r
ggplot(data=DNase) +
  geom_point(aes(conc, density))
```

![](hw_8_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

``` r
#Showing conc and density are not related in a linear way.
```
