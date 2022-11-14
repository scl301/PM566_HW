Assignment 4: HPC and SQL
================
Stephanie Lee
2022-11-14

# HPC

## Problem 1: Make sure code is nice

``` r
# Use the data with this code
set.seed(2315)
dat <- matrix(rnorm(200 * 100), nrow = 200)
```

``` r
# Total row sums
fun1 <- function(mat) {
  n <- nrow(mat)
  ans <- double(n) 
  for (i in 1:n) {
    ans[i] <- sum(mat[i, ])
  }
  ans
}

fun1(dat)
```

``` r
fun1alt <- function(mat) {
  ans <- rowSums(mat)
  ans
}

fun1alt(dat)
```

``` r
# Test for the first
microbenchmark::microbenchmark(
  fun1(dat),
  fun1alt(dat), check = "equivalent"
)
```

    ## Unit: microseconds
    ##          expr min     lq    mean median    uq    max neval
    ##     fun1(dat) 342 356.15 466.418 401.45 512.8 1517.1   100
    ##  fun1alt(dat)  51  55.30  68.909  67.85  76.1  192.2   100

``` r
# Cumulative sum by row
fun2 <- function(mat) {
  n <- nrow(mat)
  k <- ncol(mat)
  ans <- mat
  for (i in 1:n) {
    for (j in 2:k) {
      ans[i,j] <- mat[i, j] + ans[i, j - 1]
    }
  }
  ans
}

fun2(dat)
```

``` r
fun2alt <- function(mat) {
  ans <- t(apply(dat, 1, cumsum))
  ans
}

fun2alt(dat)
```

``` r
# Test for the second
microbenchmark::microbenchmark(
  fun2(dat),
  fun2alt(dat), check = "equivalent"
)
```

    ## Unit: microseconds
    ##          expr    min      lq     mean  median     uq     max neval
    ##     fun2(dat) 2434.9 2476.55 4054.538 3244.15 5146.6 13351.9   100
    ##  fun2alt(dat)  651.5  690.70 1069.369  811.40 1176.1  7134.6   100

## Problem 2: parallel computing

``` r
sim_pi <- function(n = 1000, i = NULL) {
  p <- matrix(runif(n*2), ncol = 2)
  mean(rowSums(p^2) < 1) * 4
}

# Here is an example of the run
set.seed(156)
sim_pi(1000) # 3.132
```

    ## [1] 3.132

``` r
# This runs the simulation a 4,000 times, each with 10,000 points
set.seed(1231)
system.time({
  ans <- unlist(lapply(1:4000, sim_pi, n = 10000))
  print(mean(ans))
})
```

    ## [1] 3.14124

    ##    user  system elapsed 
    ##    3.61    0.00    3.71

``` r
system.time({
  cl <- makePSOCKcluster(4)
  clusterSetRNGStream(cl, 1231)
  ans <- unlist(parLapply(cl, 1:4000, sim_pi, n = 10000))
  print(mean(ans))
  stopCluster(cl)
})
```

    ## [1] 3.141578

    ##    user  system elapsed 
    ##    0.00    0.05    2.61

## Problem 3
