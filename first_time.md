# Things to do after first installiation of Linux

## General

## R

### Set up the .Rprofile
After the first installion in R you need to set up the .Rprofile. First check if there is a .Rprofile for the entire system with

```r
site_path = R.home(component = "home")
fname = file.path(site_path, "etc", "Rprofile.site")
file.exists(fname) # TRUE IF THE FILE ALREADY EXISTS
file.edit(fname) #IF TRUE WITH THIS COMMAND YOU CAN EDIT THE EXISTING .Rprofile
```

The just copy the following file
