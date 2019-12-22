# Things to do after first installiation of Linux

## General

Rember to install the following programs:

* Dropbox
* R and Rstudio
* [TinyTex](https://yihui.name/tinytex/) (through R package)
* LyX
* TeXstudio
* Spotify
* Okular
* Gnome tweak tool
* Skype

## R and RStudio

### Set up the .Rprofile
After the first installion in R you need to set up the .Rprofile. First check if there is a .Rprofile for the entire system with

```r
site_path = R.home(component = "home")
fname = file.path(site_path, "etc", "Rprofile.site")
file.exists(fname) # TRUE IF THE FILE ALREADY EXISTS
file.edit(fname) #IF TRUE WITH THIS COMMAND YOU CAN EDIT THE EXISTING .Rprofile
```

The just copy the following inside the `.Rpfoile`

```r
# Welcome message 

message("Hello Claudio, Welcome to R")

# Custom options

options(continue = " ")

# Setting the CRAN mirror

local({
  r = getOption("repos")             
  r["CRAN"] = "https://cran.rstudio.com/"
  options(repos = r)
})

# Fortunes 

if(interactive()) 
  try(fortunes::fortune(), silent = TRUE)

.Last = function() {
  cond = suppressWarnings(!require(fortunes, quietly = TRUE))
  if(cond) 
    try(install.packages("fortunes"), silent = TRUE)
  message("Goodbye at ", date(), "\n")
}

# Nice plotting window

nice_par = function(mar = c(3, 3, 2, 1), mgp = c(2, 0.4, 0), tck = -0.01, 
                      cex.axis = 0.9, las = 1, mfrow = c(1, 1), ...) {
    par(mar = mar, mgp = mgp, tck = tck, cex.axis = cex.axis, las = las, 
        mfrow = mfrow, ...)
}
```

### Change the BLAS libraries to make R faster
To do this just follow the following [turorial](https://github.com/claudiofronterre/misc/blob/master/openBLAS.md).

### RStudio options
* `Restore .RData`: Unticking this default preventing loading previously creating R objects. This will make starting R quicker and also reduce the chance of getting bugs due to previously created objects. For this reason we recommend you untick this box.
* `Save workspcace to .RData on exit`: change to Never. 
* Go to `Appearance`: set Editor theme `Solarized Dark`. From time to time check if there is something nicer. 

### Configure git
Follow the steps in [Happy git with R.](https://happygitwithr.com/index.html). Here is a brief summary. If `git` is not installed already install it using the following code:

**Ubuntu**
```
sudo apt-get install git
```

**Mac OS**

Install [Homebrew](http://brew.sh), "the missing package manager for OS X". Among many other things, it can install Git for you. Once you have Homebrew installed, do this in the shell:

```
brew install git
```

Now we need to introduce ourself to git. In the shell type:

``` bash
git config --global user.name 'Claudio Fronterre'
git config --global user.email 'cla.f90@gmail.com'
git config --global --list
```

substituting your name and **the email associated with your GitHub account**.

The [usethis package](https://usethis.r-lib.org) offers an alternative approach. You can set your Git user name and email from within R:

```r
## install if needed (do this exactly once):
## install.packages("usethis")
library(usethis)
use_git_config(user.name = "Claudio Fronterre", user.email = "cla.f90@gmail.com")
```
