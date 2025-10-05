Iteration and listcols
================

## Lists

You can put anything in a list

``` r
vec_numeric = 5:8
vec_char = c("My", "name", "is", "Jeff")
vec_logical = c(TRUE, TRUE, TRUE, FALSE)
```

``` r
l = list(
  vec_numeric = 5:8,
  mat         = matrix(1:8, 2, 4),
  vec_logical = c(TRUE, FALSE),
  summary     = summary(rnorm(1000)))
l
```

    ## $vec_numeric
    ## [1] 5 6 7 8
    ## 
    ## $mat
    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    3    5    7
    ## [2,]    2    4    6    8
    ## 
    ## $vec_logical
    ## [1]  TRUE FALSE
    ## 
    ## $summary
    ##     Min.  1st Qu.   Median     Mean  3rd Qu.     Max. 
    ## -3.28024 -0.69805  0.02620  0.01294  0.76118  3.36686

``` r
l$vec_numeric
```

    ## [1] 5 6 7 8

``` r
l[[1]]
```

    ## [1] 5 6 7 8

``` r
l[[1]][1:3]
```

    ## [1] 5 6 7

## `for` loop

Create a new list.

``` r
list_norms = 
  list(
    a = rnorm(20, 3, 1),
    b = rnorm(20, 0, 5),
    c = rnorm(20, 10, .2),
    d = rnorm(20, -3, 1)
  )

is.list(list_norms)
```

    ## [1] TRUE

Pause and get my old function.

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

I can apply that function to each list element.

``` r
mean_and_sd(list_norms[[1]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.97 0.803

``` r
mean_and_sd(list_norms[[2]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 0.566  4.23

``` r
mean_and_sd(list_norms[[3]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.1 0.222

``` r
mean_and_sd(list_norms[[4]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.86  1.23

Let’s use a for loop:

``` r
output = vector("list", length = 4)

for (i in 1:4) {
  output[[i]] = mean_and_sd(list_norms[[i]])
}
```

## Let’s try map!

``` r
output = map(list_norms, mean_and_sd)
```

what if you want a different function…?

``` r
output = map_dbl(list_norms, median, .id = "input")
```

``` r
output = map_dfr(list_norms, mean_and_sd, .id = "input")
```

``` r
output = map2(input_1, input_2, \(x,y) func(arg_1 = x, arg_2 = y))
```

    ## Error: object 'input_1' not found

## List columns

``` r
listcol_df = 
  tibble(
    name = c("a", "b", "c", "d"),
    samp = list_norms
  )
```

``` r
listcol_df |> pull(name)
```

    ## [1] "a" "b" "c" "d"

``` r
listcol_df |> pull(samp)
```

    ## $a
    ##  [1] 3.714688 2.915657 2.655510 4.213158 2.121730 3.485953 2.999811 2.735931
    ##  [9] 2.599486 3.266211 3.855262 1.351308 2.961393 2.021249 2.483867 2.654023
    ## [17] 4.716224 2.255450 2.662017 3.667875
    ## 
    ## $b
    ##  [1]  5.7937143 -4.4468809  0.4663956  2.8532794 -3.4058472  3.2370397
    ##  [7] -6.2665154  2.3061872  4.7313217  3.2567761  2.9355288 -8.4541056
    ## [13] -4.0765023  0.7063855  6.4267365  0.7053881  3.9861499  2.1893052
    ## [19]  2.6670976 -4.2999380
    ## 
    ## $c
    ##  [1] 10.564312 10.051142 10.219290 10.125271  9.755923 10.185012  9.999865
    ##  [8]  9.887744 10.235834  9.856306 10.032594 10.137871  9.554127 10.196710
    ## [15] 10.172675 10.147352 10.055779  9.795881 10.010470 10.310685
    ## 
    ## $d
    ##  [1] -3.215312 -3.740919 -4.515961 -2.997837 -1.234467 -3.563570 -2.445091
    ##  [8] -2.739837 -1.709039 -4.023575 -1.175354 -1.590038 -3.330464 -2.295832
    ## [15] -3.504192 -1.395825 -1.950707 -5.394976 -1.698280 -4.605313

``` r
listcol_df$samp[[1]]
```

    ##  [1] 3.714688 2.915657 2.655510 4.213158 2.121730 3.485953 2.999811 2.735931
    ##  [9] 2.599486 3.266211 3.855262 1.351308 2.961393 2.021249 2.483867 2.654023
    ## [17] 4.716224 2.255450 2.662017 3.667875

Lets’ try some operations

``` r
mean_and_sd(listcol_df$samp[[1]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.97 0.803

Can I just … map?

``` r
map(listcol_df$samp, mean_and_sd)
```

    ## $a
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.97 0.803
    ## 
    ## $b
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 0.566  4.23
    ## 
    ## $c
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.1 0.222
    ## 
    ## $d
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.86  1.23

So … can I add a list column?

``` r
listcol_df = 
  listcol_df |> 
  mutate(summary = map(samp, mean_and_sd))

listcol_df
```

    ## # A tibble: 4 × 3
    ##   name  samp         summary         
    ##   <chr> <named list> <named list>    
    ## 1 a     <dbl [20]>   <tibble [1 × 2]>
    ## 2 b     <dbl [20]>   <tibble [1 × 2]>
    ## 3 c     <dbl [20]>   <tibble [1 × 2]>
    ## 4 d     <dbl [20]>   <tibble [1 × 2]>

``` r
nsduh_table <- function(html, table_num) {
  
  table = 
    html |> 
    html_table() |> 
    nth(table_num) |>
    slice(-1) |> 
    select(-contains("P Value")) |>
    pivot_longer(
      -State,
      names_to = "age_year", 
      values_to = "percent") |>
    separate(age_year, into = c("age", "year"), sep = "\\(") |>
    mutate(
      year = str_replace(year, "\\)", ""),
      percent = str_replace(percent, "[a-c]$", ""),
      percent = as.numeric(percent)) |>
    filter(!(State %in% c("Total U.S.", "Northeast", "Midwest", "South", "West")))
  
  table
}
```
