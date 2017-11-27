# Useful functions in R that I always forget :)

## Tricks for Dates

#### Converting Excel numeric dates in Date class 
Excel is said to use 1900-01-01 as day 1 (Windows default) or 1904-01-01 as day 0 (Mac default), but this is complicated by Excel incorrectly treating 1900 as a leap year. So for dates (post-1901) from Windows Excel

```r
as.Date(35981, origin = "1899-12-30") 
1998-07-05 #Result
```

#### Converting year-month-day Date to Mont Year format 
Then if you want to convert them in a Month Year format you can use

```r
zoo::as.yearmon(x)
```

where `x` must be of class `Date`.

#### Genereting monthly harmoninc terms from a Date object
This code is useful for generenti sine cosine terms from a Date class object

```r
df$month <- month(df$date) + (as.numeric(as.factor(year(df$date))) - 1)*12 
for (i in c(1, 2, 3)) {
  df[paste0("s", i)] <- sin(i * 2 * pi * df$month / 12)
  df[paste0("c", i)] <- cos(i * 2 * pi * df$month / 12)
 }
```

## Tricks for formula objects

```r
as.formula(paste("outcome", paste(names(covariates), collapse= "+"), sep = "~"))
```

