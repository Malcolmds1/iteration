Writing functions
================

## Do something simple

``` r
x_vec = rnorm(25, mean = 5, sd = 3)

(x_vec - mean(x_vec)) / sd(x_vec)
```

    ##  [1] -1.101388515  0.267488264  0.828434957 -0.398422095  1.687853548
    ##  [6]  0.294857516  0.844921699 -0.869469807  0.894645904  0.101169173
    ## [11] -0.510047267  0.008648538  0.971431285  0.123347990 -2.059310076
    ## [16]  0.503493242 -2.183007615  0.452322512 -0.104006807 -1.301781469
    ## [21]  1.400105035  0.812838020  0.203115292  0.349580003 -1.216819326

I want a function to compute z-scores

``` r
z_scores = function(x) {
  
  z = (x - mean(x)) / sd(x)
  z
  
}

z_scores(x_vec)
```

    ##  [1] -1.101388515  0.267488264  0.828434957 -0.398422095  1.687853548
    ##  [6]  0.294857516  0.844921699 -0.869469807  0.894645904  0.101169173
    ## [11] -0.510047267  0.008648538  0.971431285  0.123347990 -2.059310076
    ## [16]  0.503493242 -2.183007615  0.452322512 -0.104006807 -1.301781469
    ## [21]  1.400105035  0.812838020  0.203115292  0.349580003 -1.216819326

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

    ##  [1]  1.3063945  1.3063945  1.3063945 -0.7348469 -0.7348469 -0.7348469
    ##  [7]  1.3063945  1.3063945  1.3063945 -0.7348469 -0.7348469 -0.7348469
    ## [13] -0.7348469 -0.7348469 -0.7348469 -0.7348469 -0.7348469 -0.7348469
    ## [19] -0.7348469 -0.7348469  1.3063945 -0.7348469 -0.7348469  1.3063945
    ## [25]  1.3063945

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
