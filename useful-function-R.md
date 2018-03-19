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

## Tricks for spatial data

### Extract values from many rasters with different resolution and extensions

This function extract values from a group of rasters given a set of points. It also allows the presence of rasters with differnt extentions, resolution and projections. It has the following arguments

* `path` the folder containing the rasters. It will also check in the subfolderds.
* `pattern` the file extension of the rasters. The default is tif files.
* `coords` points represented by a two-column matrix or data.frame, or a SpatialPoints object. 
* `check.packages` to facilitate reproducibility, when this argument is set to TRUE it will load (and install if missing) all the packages needed by the function to work properly (raster, sp and pbapply).

The function checks also the consistency between the procjection of the raster and the points provided. If the projection is different the points are converted to the same projection of the raster. It returns a `data.frame` object with each column containing the extracted values from each raster. 

```r
extract_raster <- function(path = getwd(), pattern = ".tif$", coords, check.pacakges = FALSE) {
  if(check.pacakges == TRUE) {
  if (!require("pacman")) install.packages("pacman")
  pkgs = c("raster", "sp", "pbapply")
  pacman::p_load(pkgs, character.only = T)
  }
  raster_list <- list.files(path = path, recursive = T, pattern = ".tif$", full.names = TRUE)
  col_names <- tools::file_path_sans_ext(basename(raster_list))
  df <- pbsapply(raster_list, FUN = function(x) {
    temp <- stack(x) 
    if (proj4string(temp) != proj4string(coords)) coords <- spTransform(coords, CRSobj = crs(temp))
    df <- extract(temp, coords)
    })
  df <- as.data.frame(df)
  names(df) <- col_names
  return(df)
} 
```

### Coordinate reference systems

It is more convenient to use EPSG codes when doing reprojections. The codes that I mainly use are:

1. epsg 4326 that corresponds to GS84 lonlat projection.
2. epsg 3857 for Web Mercator (Auxiliary Sphere) projection (this is good for palces that pass through more UTM zone).

For country scale and or more local analysis UTM projection is more precise for distances but we need to define a specific zone. 

## Various

When loading a lot of r packages use this:

```r
if (!require("pacman")) install.packages("pacman") #install pacman library if missing
pkgs = c("rstan", "shinystan", "sp", "geoR", "MBA", "tmap", "spBayes", "fields") # package names
pacman::p_load(pkgs, character.only = T)
```

It will also install missing packages.

To create categories for prevalence that contains a category with only zeros do the following

```r
prev <- cut(onco$prevalence, breaks = seq(0, 1, by = 0.2)) #equal categories open at left and closed at right
prev <- forcats::fct_explicit_na(prev, na_level = "0") #converts the na to 0
prev <- forcats::fct_shift(prev, -1) # reoder the levels
```
