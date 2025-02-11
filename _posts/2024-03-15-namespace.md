---
title: 'Stop using the library() function'
date: 2024-03-15
permalink: /posts/2024/03/namespace/
tags:
  - R
---

Yes, i know, you love using the `library()` function in R! But it can be a real problem for several reasons:

* It makes your code too ambiguous 
* It can mask other functions with similar names, if you have multiple library statements
* It is forbidden in R package creations

Let's take each one of these points in turn

### Ambiguous code

When you library multiple packages, it becomes unclear which functions belong to which packages. This makes your code really difficult to read for other users, especially if you request help

### Masking/ name clashes

When you library multiple packages, if any of the packages contain functions with the same name, or contain functions with the same name as functions in packages bundled with R (like `stats` or `utils`) then all but one will be masked. The order in which packages are libraried matters. The most recent one will take precedence

Consider this example

``` r
library(dplyr)
#> 
#> Attaching package: 'dplyr'
#> The following objects are masked from 'package:stats':
#> 
#>     filter, lag
#> The following objects are masked from 'package:base':
#> 
#>     intersect, setdiff, setequal, union
```

<sup>Created on 2025-02-10 with [reprex v2.1.0](https://reprex.tidyverse.org)</sup>

Just this one `library` command has masked a whole bunch of functions from base R.

### Creating R packages

Eventually, you will evolve as an R coder to realizing that you want/need to write an R package. At this point, the above points will make more sense (if they do not already).

When writing R functions you are not permitted to use the `library` command in any function. You must, instead, reference every function using the package they come from (i.e their `namespace`). This is achieved using the `::` operator.

For example 

```r
iris |> 
  tidyr::pivot_longer(cols = -Species,
                      names_to = "names",
                      values_to = "values") |> 
  dplyr::group_by(Species,names) |> 
  dplyr::summarise(mn=base::mean(values),
                   .groups = "drop")

```

Here, the `pivot_longer` function comes from the `tidyr` package, `summarize` and `group_by` come from the `dplyr` package and `mean` comes from `base`. 

In conclusion: Yes, you do end up typing a lot more but overall it's a good practice!
