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
    ##          expr     min       lq      mean  median       uq     max neval
    ##     fun1(dat) 245.701 351.0505 402.82293 362.000 438.3005 819.602   100
    ##  fun1alt(dat)  51.001  52.9505  60.95894  54.901  64.3005 133.301   100

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
    ##          expr      min       lq     mean   median       uq       max neval
    ##     fun2(dat) 2397.601 3084.052 5331.156 5066.101 6521.151 12880.002   100
    ##  fun2alt(dat)  658.701  757.051 1313.981 1354.101 1493.901  7304.001   100

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
    ##    3.89    0.02    4.01

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
    ##    0.01    0.02    2.41

# SQL

``` r
# install.packages(c("RSQLite", "DBI"))

library(RSQLite)
library(DBI)

# Initialize a temporary in memory database
con <- dbConnect(SQLite(), ":memory:")

# Download tables
film <- read.csv("https://raw.githubusercontent.com/ivanceras/sakila/master/csv-sakila-db/film.csv")
film_category <- read.csv("https://raw.githubusercontent.com/ivanceras/sakila/master/csv-sakila-db/film_category.csv")
category <- read.csv("https://raw.githubusercontent.com/ivanceras/sakila/master/csv-sakila-db/category.csv")

# Copy data.frames to database
dbWriteTable(con, "film", film)
dbWriteTable(con, "film_category", film_category)
dbWriteTable(con, "category", category)
```

## Question 1

How many movies are there available in each rating catagory.

``` sql
SELECT rating, COUNT(*) AS count
FROM film
GROUP BY rating
```

| rating | count |
|:-------|------:|
| G      |   180 |
| NC-17  |   210 |
| PG     |   194 |
| PG-13  |   223 |
| R      |   195 |

5 records

## Question 2

What is the average replacement cost and rental rate for each rating
category.

``` sql
SELECT c.category_id, AVG(f.replacement_cost) AS avg_replacement_cost, f.rental_rate 
FROM film AS f 
  INNER JOIN film_category AS c
    ON f.film_id = c.film_id
GROUP BY c.category_id
```

| category_id | avg_replacement_cost | rental_rate |
|:------------|---------------------:|------------:|
| 1           |             20.91187 |        0.99 |
| 2           |             20.12636 |        0.99 |
| 3           |             20.05667 |        4.99 |
| 4           |             21.00754 |        0.99 |
| 5           |             19.02448 |        4.99 |
| 6           |             19.62235 |        0.99 |
| 7           |             21.08677 |        2.99 |
| 8           |             19.72913 |        2.99 |
| 9           |             18.64753 |        2.99 |
| 10          |             20.28508 |        4.99 |

Displaying records 1 - 10

## Question 3

Use table film_category together with film to find the how many films
there are within each category ID

``` sql
SELECT c.category_id, COUNT(*) AS count
FROM film_category AS c 
  LEFT JOIN film AS f
    ON f.film_id = c.film_id
GROUP BY c.category_id
```

| category_id | count |
|:------------|------:|
| 1           |    64 |
| 2           |    66 |
| 3           |    60 |
| 4           |    57 |
| 5           |    58 |
| 6           |    68 |
| 7           |    62 |
| 8           |    69 |
| 9           |    73 |
| 10          |    61 |

Displaying records 1 - 10

## Question 4

Incorporate table category into the answer to the previous question to
find the name of the most popular category.

``` sql
SELECT c.name, fc.category_id, COUNT(*) AS count
FROM film_category AS fc
  LEFT JOIN film AS f 
    ON f.film_id = fc.film_id
      JOIN category AS c
        ON fc.category_id = c.category_id
GROUP BY c.category_id
ORDER BY count DESC
```

| name        | category_id | count |
|:------------|------------:|------:|
| Sports      |          15 |    74 |
| Foreign     |           9 |    73 |
| Family      |           8 |    69 |
| Documentary |           6 |    68 |
| Animation   |           2 |    66 |
| Action      |           1 |    64 |
| New         |          13 |    63 |
| Drama       |           7 |    62 |
| Sci-Fi      |          14 |    61 |
| Games       |          10 |    61 |

Displaying records 1 - 10

The most popular category is Sports with the highest number of films.
