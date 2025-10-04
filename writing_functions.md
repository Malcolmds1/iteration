Writing functions
================

## Do something simple

``` r
x_vec = rnorm(25, mean = 5, sd = 3)

(x_vec - mean(x_vec)) / sd(x_vec)
```

    ##  [1]  0.5069151  0.3318885 -0.4043052  0.2499178 -2.3357441 -1.6096828
    ##  [7]  0.1274479  0.8120381 -0.6314062 -1.7514353  1.0623805  1.3466070
    ## [13]  1.2443990  0.1512704  1.2620192  0.5594553 -0.4321241 -1.2629327
    ## [19] -0.5374174  0.9411558 -0.4929008  0.4216235 -0.2760764 -0.3794509
    ## [25]  1.0963579

I want a function to compute z-scores

``` r
z_scores = function(x) {
  
  z = (x - mean(x)) / sd(x)
  z
  
}

z_scores(x_vec)
```

    ##  [1]  0.5069151  0.3318885 -0.4043052  0.2499178 -2.3357441 -1.6096828
    ##  [7]  0.1274479  0.8120381 -0.6314062 -1.7514353  1.0623805  1.3466070
    ## [13]  1.2443990  0.1512704  1.2620192  0.5594553 -0.4321241 -1.2629327
    ## [19] -0.5374174  0.9411558 -0.4929008  0.4216235 -0.2760764 -0.3794509
    ## [25]  1.0963579

Try my function on some other thing. These should give errors

``` r
z_score(3)
```

    ## Error in z_score(3): could not find function "z_score"

``` r
z_scores("my name is malcolm")
```

    ## Warning in mean.default(x): argument is not numeric or logical: returning NA

    ## Error in x - mean(x): non-numeric argument to binary operator

``` r
z_scores(iris)
```

    ## Warning in mean.default(x): argument is not numeric or logical: returning NA

    ## Warning in Ops.factor(left, right): '-' not meaningful for factors

    ## Error in is.data.frame(x): 'list' object cannot be coerced to type 'double'

``` r
z_scores(sample(c(TRUE, FALSE), 25, replace = TRUE))
```

    ##  [1] -1.2  0.8  0.8 -1.2 -1.2  0.8 -1.2 -1.2  0.8  0.8  0.8  0.8 -1.2 -1.2  0.8
    ## [16] -1.2  0.8  0.8  0.8  0.8 -1.2  0.8  0.8 -1.2  0.8

``` r
z_scores = function(x) {
  
  if (!is.numeric(x)) {
    stop("Argument x should be numeric")
  } else if (length(x) == 1) {
    stop("Z scores cannot be computed for length 1 vectors")
  }
  
  z = mean(x) / sd(x)
  
  z
}
```

## Multiple outputs

``` r
mean_and_sd = function(x) {
  
  if (!is.numeric(x)) {
    stop("Argument x should be numeric")
  } else if (length(x) == 1) {
    stop("Cannot be computed for length 1 vectors")
  }
  
  mean_x = mean(x)
  sd_x = sd(x)

  list(mean = mean_x, 
       sd = sd_x)
}
```

Alternatively, we might store values in a data frame.

``` r
mean_and_sd = function(x) {
  
  if (!is.numeric(x)) {
    stop("Argument x should be numeric")
  } else if (length(x) == 1) {
    stop("Cannot be computed for length 1 vectors")
  }
  
  mean_x = mean(x)
  sd_x = sd(x)

  tibble(
    mean = mean_x, 
    sd = sd_x
  )
}
```

## Multiple inputs

``` r
sim_data = tibble(
  x = rnorm(30, mean = 2, sd = 3)
)

sim_data |> 
  summarize(
    mu_hat = mean(x),
    sigma_hat = sd(x)
  )
```

    ## # A tibble: 1 × 2
    ##   mu_hat sigma_hat
    ##    <dbl>     <dbl>
    ## 1   1.81      2.14

``` r
sim_mean_sd = function(n, mu = 2, sigma = 3) {
  
  sim_data = tibble(
    x = rnorm(n, mean = mu, sd = sigma),
  )
  
  sim_data |> 
    summarize(
      mu_hat = mean(x),
      sigma_hat = sd(x)
    )
}
```
