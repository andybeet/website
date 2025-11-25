---
title: 'Error handling with `tryCatch()`'
date: 2024-10-01
permalink: /posts/2024/10/trycatch/
tags:
  - R
---

Error handling is not something scientists typically incorporate into their code. However, there a situations in which incorporating such a thing can save you hours of headache! 

Some examples:

* You want to run a lot of model simulations/fitting in sequence. If one model iteration causes an error then your code stops without finishing
* You are pulling a large amount of data from a server to analyse. Connection to the server is interrupted and your workflow stops.
* Generally, you have a series of steps in workflow. If one of the steps fail, the whole workflow fails.

Ideally, you want to be able to handle these situations within your code, adapt to the error, and continue.

In the examples above, solutions might be:

* If a model causes an error, write the issue to a file, then simulate a replacement.
* If the server connection is interrupted, and throws an error, try to reconnect, before continuing 

Using the `tryCatch()` function, bundled in base R, is a great option.

## Example 1: Database connection

This example attempts to accesses an oracle server to pull data. 
Custom warnings are returned based on the type of warning thrown
    
```    
chan <- tryCatch(
  {
    driver <- ROracle::Oracle()
    chan <- ROracle::dbConnect(driver, dbname=server,username=uid,password=pwd)
  }, warning=function(w) {
    if (grepl("login denied",w)) {message("login to server failed - Check username and password")}
    if (grepl("locked",w)) {message("logon to server failed - Account may be locked")}
    message(paste0("Can not Connect to Database: ",server))
    return(NA)
  }, error=function(e) {
    message(paste0("Terminal error: ",e))
    return(NA)
  }
)

```

## Example 2: Download a file from a url 

Attempt to download a file from an online location. If it's missing, or can't be downloaded for some reason, the code skips the file


```
# get file, catch error for missing file
result <- tryCatch(
  {
    downloader::download(fpath,destfile=destfile,quiet=TRUE,mode="wb")
    res <- TRUE
  }, error = function(e){
    message(paste0("No data for ",afname))
    return(FALSE)
  },warning = function(w) 
    return(FALSE)
)

if (!result) {
  next
}
```

In summary, error handling can save you a lot of frustration from code breaking prematurely!
