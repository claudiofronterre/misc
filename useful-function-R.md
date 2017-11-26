# Useful functions in R that I always forget :)

## Tricks for Dates
Excel is said to use 1900-01-01 as day 1 (Windows default) or 1904-01-01 as day 0 (Mac default), but this is complicated by Excel incorrectly treating 1900 as a leap year. So for dates (post-1901) from Windows Excel

```r
as.Date(35981, origin = "1899-12-30") 
1998-07-05 #Result
```

Then if you want to convert them in a Month Year format you can use

```r
zoo::as.yearmon(x)
```

where `x` must be of class `Date`.
