Homework 8
================

### Question 1

Plotting the relationship of price as a function of weight for each
color in order of decreasing slope for the *diamonds* data set.

``` r
library(tidyverse)
library(modelr)

ggplot(data = diamonds) +
  geom_point(aes(x=carat, y=price, color = color))
```

![](hw_8_files/figure-gfm/unnamed-chunk-1-1.png)<!-- -->

``` r
# plotting general relationship between carat and price for the whole data set

dmd <- diamonds %>%
  group_by(color) %>%
  nest()

# grouping by color and listing the data via nest
```

``` r
color_model <- function(df)
  lm(price~carat, data = df)
# Creating the linear model and primary function df

dmd <- dmd %>%
  mutate(model = map(data, color_model))

dmd <- dmd %>%
  mutate(predicts = map2(data, model, add_predictions))

predicts <- unnest(dmd, predicts)

# unnesting the list in order to plot the data
```

``` r
ggplot(data = predicts) +
  geom_line(aes(x=carat, y=pred)) +
  geom_point(aes(x=carat, y=price)) +
  facet_wrap(~color)
```

![](hw_8_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

``` r
arrange(predicts, desc(color))
```

    ## # A tibble: 53,940 × 13
    ## # Groups:   color [7]
    ##    color data     model  carat cut   clarity depth table price     x     y     z
    ##    <ord> <list>   <list> <dbl> <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ##  1 J     <tibble… <lm>    0.31 Good  SI2      63.3    58   335  4.34  4.35  2.75
    ##  2 J     <tibble… <lm>    0.24 Very… VVS2     62.8    57   336  3.94  3.96  2.48
    ##  3 J     <tibble… <lm>    0.3  Good  SI1      64      55   339  4.25  4.28  2.73
    ##  4 J     <tibble… <lm>    0.23 Ideal VS1      62.8    56   340  3.93  3.9   2.46
    ##  5 J     <tibble… <lm>    0.31 Ideal SI2      62.2    54   344  4.35  4.37  2.71
    ##  6 J     <tibble… <lm>    0.3  Good  SI1      63.4    54   351  4.23  4.29  2.7 
    ##  7 J     <tibble… <lm>    0.3  Good  SI1      63.8    56   351  4.23  4.26  2.71
    ##  8 J     <tibble… <lm>    0.3  Very… SI1      62.7    59   351  4.21  4.27  2.66
    ##  9 J     <tibble… <lm>    0.31 Very… SI1      59.4    62   353  4.39  4.43  2.62
    ## 10 J     <tibble… <lm>    0.31 Very… SI1      58.1    62   353  4.44  4.47  2.59
    ## # … with 53,930 more rows, and 1 more variable: pred <dbl>

### Question 2

Fitting nonlinear models using either a monod model or a single square
root model and comparing the effectiveness of both at displaying the
DNase data set.

``` r
library(tidyverse)

ggplot(data=DNase) +
  geom_point(aes(conc, density))
```

![](hw_8_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

``` r
# Showing conc and density are not related in a linear way.
```

``` r
library(nls2)
library(proto)

nls_mod <- formula(density ~ beta_1 * sqrt(conc) + beta_0)

single_sqrt_model <- nls2(nls_mod, data = DNase, start = list(beta_1 =0.5, beta_0 = 0.1))

summary(single_sqrt_model)
```

    ## 
    ## Formula: density ~ beta_1 * sqrt(conc) + beta_0
    ## 
    ## Parameters:
    ##         Estimate Std. Error t value Pr(>|t|)    
    ## beta_1  0.550521   0.006287   87.57  < 2e-16 ***
    ## beta_0 -0.053297   0.011081   -4.81 3.26e-06 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.08897 on 174 degrees of freedom
    ## 
    ## Number of iterations to convergence: 1 
    ## Achieved convergence tolerance: 6.129e-08

``` r
# Creating the square root model
```

``` r
max(DNase$density)
```

    ## [1] 2.003

``` r
nls_mod2 <- formula(density ~ (conc * Dmax)/(conc + k))

monod_model <- nls2(nls_mod2, data = DNase, start = list(Dmax = 2.003, k = 2.003/2))

summary(monod_model)
```

    ## 
    ## Formula: density ~ (conc * Dmax)/(conc + k)
    ## 
    ## Parameters:
    ##      Estimate Std. Error t value Pr(>|t|)    
    ## Dmax  2.28032    0.02189  104.16   <2e-16 ***
    ## k     3.68241    0.08677   42.44   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.04909 on 174 degrees of freedom
    ## 
    ## Number of iterations to convergence: 6 
    ## Achieved convergence tolerance: 2.375e-06

``` r
# Creating the monod model and defining the variables Dmax and K
```

``` r
glimpse(DNase)
```

    ## Rows: 176
    ## Columns: 3
    ## $ Run     <ord> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 2, 2, 2, 2,…
    ## $ conc    <dbl> 0.04882812, 0.04882812, 0.19531250, 0.19531250, 0.39062500, 0.…
    ## $ density <dbl> 0.017, 0.018, 0.121, 0.124, 0.206, 0.215, 0.377, 0.374, 0.614,…

``` r
DNaseR <- DNase%>%
  group_by(Run) %>%
  nest()
# Generating a list with each of the 11 runs grouped.

                
#Applied calculated formulas across grouped values.
```

I could not figure out how to apply the formulas across the nested lists
by the deadline. I will continue to trouble shoot the problem and
resubmit as soon as possible.
