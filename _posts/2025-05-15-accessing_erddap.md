---
title: 'Pulling data from ERDDAP™'
date: 2025-05-15
permalink: /posts/2025/05/pull_from_erddap/
tags:
  - R
---

Many publicly available scientific datasets from around the world can be found on servers supported by ERDDAP™ (Environmental Research Division's Data Access Program). Many organizations, like government agencies and universities, run their own ERDDAP™ servers and host oceanographic and environmental datasets. 

The data are served up in a consistent way, allowing users to visualize and download data in various standard data formats, like netCDF (nc), csv, txt, JSON.

While it is perfectly reasonable to interact with these ERDDAP™ servers through a web interface, and often preferable initially to identify data sources, read metadata, and visualize the data, pulling and working with the data is often best accomplished using a language like python or R. Since i mostly use R for my work, we'll go that route.

For R users there is a wonderful package called [`rerddap`](https://docs.ropensci.org/rerddap/) designed to help search, connect, and download data from ERDDAP™ servers. Lets go through an example to demonstrate how to get started. The steps involved are

* List all servers (`rerddap::servers()`)
* List all datasets on a specific server (`rerddap::ed_datasets()`)
* Select and explore dataset fields (`rerddap::info()`)
* Pull data (`rerddap::tabledap()` or `rerddap::griddap()`)

## List servers

First, let's see the list of server names available using the `servers()` function

``` r
rerddap::servers()
#> # A tibble: 63 × 4
#>    name                                                  short_name url   public
#>    <chr>                                                 <chr>      <chr> <lgl> 
#>  1 Voice of the Ocean                                    VOTO       http… TRUE  
#>  2 St. Lawrence Global Observatory - CIOOS | Observatoi… SLGO-OGSL  http… TRUE  
#>  3 CoastWatch West Coast Node                            CSWC       http… TRUE  
#>  4 ERDDAP at the Asia-Pacific Data-Research Center       APDRC      http… TRUE  
#>  5 NOAA's National Centers for Environmental Informatio… NCEI       http… TRUE  
#>  6 Biological and Chemical Oceanography Data Management… BCODMO     http… TRUE  
#>  7 European Marine Observation and Data Network (EMODne… EMODnet    http… TRUE  
#>  8 European Marine Observation and Data Network (EMODne… EMODnet P… http… TRUE  
#>  9 Marine Institute - Ireland                            MII        http… TRUE  
#> 10 CoastWatch Caribbean/Gulf of Mexico Node              CSCGOM     http… TRUE  
#> # ℹ 53 more rows
```

<sup>Created on 2025-12-08 with [reprex v2.1.1](https://reprex.tidyverse.org)</sup>

## List datasets

The "CoastWatch West Coast Node" server looks interesting, lets explore that. We'll need to grab the `url` field to explore the datasets hosted on this server. 

``` r
servers <- rerddap::servers()
servers |> 
  dplyr::filter(short_name == "CSWC") |> 
  dplyr::select(url)
#> # A tibble: 1 × 1
#>   url                                     
#>   <chr>                                   
#> 1 https://coastwatch.pfeg.noaa.gov/erddap/
```

<sup>Created on 2025-12-08 with [reprex v2.1.1](https://reprex.tidyverse.org)</sup>


Now, at this point you can either copy this url into your browser and explore the contents of the server from there, or you can use `rerddap` to list the data sets. At this point we should mention that there are often two types of data hosted on these servers

* Gridded data, termed `griddap`, in NetCDF (nc) format
* Tabular data, termed `tabledap`, often in csv format

Let's search for all of the tabular data on this server. 

``` r
servers <- rerddap::servers()
url <- servers |> 
  dplyr::filter(short_name == "CSWC") |> 
  dplyr::select(url)
  
rerddap::ed_datasets(which = "tabledap", url = url)
#> # A tibble: 293 × 17
#>    griddap Subset     tabledap Make.A.Graph wms   files Accessible Title Summary
#>    <chr>   <chr>      <chr>    <chr>        <chr> <chr> <chr>      <chr> <chr>  
#>  1 ""      https://c… https:/… https://coa… ""    ""    public     * Th… "This …
#>  2 ""      https://c… https:/… https://coa… ""    "htt… public     Audi… "Audio…
#>  3 ""      https://c… https:/… https://coa… ""    "htt… public     CalC… "Hydro…
#>  4 ""      https://c… https:/… https://coa… ""    "htt… public     CalC… "Sampl…
#>  5 ""      https://c… https:/… https://coa… ""    "htt… public     CalC… "Cruis…
#>  6 ""      https://c… https:/… https://coa… ""    "htt… public     CalC… "Fish …
#>  7 ""      https://c… https:/… https://coa… ""    "htt… public     CalC… "Egg m…
#>  8 ""      https://c… https:/… https://coa… ""    "htt… public     CalC… "Fish …
#>  9 ""      https://c… https:/… https://coa… ""    "htt… public     CalC… "Size …
#> 10 ""      https://c… https:/… https://coa… ""    "htt… public     CalC… "Devel…
#> # ℹ 283 more rows
#> # ℹ 8 more variables: FGDC <chr>, ISO.19115 <chr>, Info <chr>,
#> #   Background.Info <chr>, RSS <chr>, Email <chr>, Institution <chr>,
#> #   Dataset.ID <chr>
```

<sup>Created on 2025-12-08 with [reprex v2.1.1](https://reprex.tidyverse.org)</sup>

We can see that there are 293 datasets available, each with its own url field (`tabldap`) and ID (`Dataset.ID`) and other accompanying metadata. As mentioned earlier it may be easier to look at these from within a web browser. But for the sake of this example let's just look at the `Summary`  metadata


``` r
servers <- rerddap::servers()
url <- servers |> 
  dplyr::filter(short_name == "CSWC") |> 
  dplyr::select(url)
datasets <- rerddap::ed_datasets(which = "tabledap", url = url)
datasets |> 
  dplyr::select(Dataset.ID,Summary)
#> # A tibble: 293 × 2
#>    Dataset.ID           Summary                                                 
#>    <chr>                <chr>                                                   
#>  1 allDatasets          "This dataset is a table which has a row of information…
#>  2 testTableWav         "Audio data from a local source.\n\ncdm_data_type = Oth…
#>  3 erdCalCOFINOAAhydros "Hydrographic data collected by CTD as part of CalCOFI …
#>  4 erdCalCOFIcufes      "Samples collected using the Continuous Underway Fish-E…
#>  5 erdCalCOFIcruises    "Cruises using one or more ships conducted as part of t…
#>  6 erdCalCOFIeggcnt     "Fish egg counts and standardized counts for eggs captu…
#>  7 erdCalCOFIeggstg     "Egg morphological developmental stage for eggs of sele…
#>  8 erdCalCOFIlrvcnt     "Fish larvae counts and standardized counts for eggs ca…
#>  9 erdCalCOFIlrvsiz     "Size data for selected larval fish captured in CalCOFI…
#> 10 erdCalCOFIlrvstg     "Developmental stages (yolk sac, preflexion, flexion, p…
#> # ℹ 283 more rows
```

<sup>Created on 2025-12-08 with [reprex v2.1.1](https://reprex.tidyverse.org)</sup>

## Explore dataset

Now, from experience i know there is a dataset hosted here containing data from the [National Data Buoy Center](https://www.ndbc.noaa.gov/) (NDBC). The data has a `Dataset.ID` = "cwwcNDBCMet".

``` r
datasets |> 
  dplyr::filter(grepl("NDBC",Summary)) |> 
  dplyr::pull(Dataset.ID)
  
[1] "cwwcNDBCMet"  
```
Once we have this id, we are ready to explore and pull the data. So lets grab all of the information relating to this dataset. We'll use the function [`info()`](https://docs.ropensci.org/rerddap/reference/info.html)

``` r
data_info <- rerddap::info(datasetid = "cwwcNDBCMet", url = "https://coastwatch.pfeg.noaa.gov/erddap/")
```

`data_info` is a list object containing metadata, like the url of the server, variables describing the variable names, variable type, and variable data range. This list object is what you'll pass to the function, `tabledap()` to pull the data.

## Pull the dataset

Even though we have identified the data set we want, we don't really know how much data there is! If you tried to pull all of the data at once, there is a good chance your computer will crash! So in prep, we can explore the set of available variables using a couple of methods that involve the `data_info` list object:

* [`rerddap::browse(data_info)`](https://upwell.pfeg.noaa.gov/erddap/info/cwwcNDBCMet/index.html) - will open a webpage on ERDDAP™ describing the data set

* `data_info$variables` will list the variables available and `data_info$alldata[[variableName]]` will show more detailed information about each variable.

``` r
erddap_url <- 'https://coastwatch.pfeg.noaa.gov/erddap/'
datasetid <- "cwwcNDBCMet"
data_info <- suppressMessages(rerddap::info(datasetid, url = erddap_url))
data_info$variables
#>    variable_name data_type           actual_range
#> 1            apd     float              0.0, 95.0
#> 2           atmp     float           -153.4, 50.0
#> 3            bar     float          800.7, 1198.8
#> 4           dewp     float            -99.9, 48.7
#> 5            dpd     float              0.0, 64.0
#> 6            gst     float              0.0, 75.5
#> 7       latitude     float          -55.0, 71.758
#> 8      longitude     float       -177.75, 179.001
#> 9            mwd     short                 0, 359
#> 10          ptdy     float            -13.1, 14.9
#> 11       station    String                       
#> 12          tide     float            -9.37, 6.15
#> 13          time    double 4910400.0, 1.7697828E9
#> 14           vis     float              0.0, 66.7
#> 15            wd     short                 0, 359
#> 16          wspd     float              0.0, 96.0
#> 17          wspu     float            -98.7, 97.5
#> 18          wspv     float            -98.7, 97.5
#> 19          wtmp     float            -98.7, 50.0
#> 20          wvht     float             0.0, 92.39
```

<sup>Created on 2026-01-30 with [reprex v2.1.1](https://reprex.tidyverse.org)</sup>

What jumps out here is the `station` variable and the `latitude` and `longitude` variables. We'll now use these to pull the list of stations available

``` r
erddap_url <- 'https://coastwatch.pfeg.noaa.gov/erddap/'
datasetid <- "cwwcNDBCMet"
data_info <- suppressMessages(rerddap::info(datasetid, url = erddap_url))

rerddap::tabledap(
  data_info,
  fields = c("station", "longitude", "latitude"),
  distinct = TRUE
)
#> info() output passed to x; setting base url to: https://coastwatch.pfeg.noaa.gov/erddap
#> <ERDDAP tabledap> cwwcNDBCMet
#> # A tibble: 1,329 × 3
#>    station longitude latitude
#>    <chr>       <dbl>    <dbl>
#>  1 0Y2W3       -87.3    44.8 
#>  2 18CI3       -86.9    41.7 
#>  3 20CM4       -86.5    42.1 
#>  4 23020        38.5    22.2 
#>  5 31201       -48.1   -27.7 
#>  6 32012       -85.4   -19.6 
#>  7 32301      -105.     -9.9 
#>  8 32302       -85.1   -18   
#>  9 32487       -77.7     3.52
#> 10 32488       -77.5     6.26
#> # ℹ 1,319 more rows
```

<sup>Created on 2026-01-30 with [reprex v2.1.1](https://reprex.tidyverse.org)</sup>

Now that you have identified the set of buoy stations available you can now either pull individual stations data or pull a collection of stations within a geographic region. Either of these tasks will require using the `erddap::tabledap()` function. For example:

### Get data from a single station

Select the station(s) of interest, then get the data. In this example we'll pull all of the data associated with buoy `32012`

``` r
erddap_url <- 'https://coastwatch.pfeg.noaa.gov/erddap/'
datasetid <- "cwwcNDBCMet"
data_info <- suppressMessages(rerddap::info(datasetid, url = erddap_url))

variables <- data_info$variables$variable_name

rerddap::tabledap(
  datasetid,
  fields = variables,
  query = paste0('station="', 32012, '"')
)
#> <ERDDAP tabledap> cwwcNDBCMet
#>    File size:    [9.37 mb]
#> # A tibble: 84,918 × 20
#>      apd  atmp   bar  dewp   dpd   gst latitude longitude   mwd  ptdy station
#>    <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>    <dbl>     <dbl> <int> <dbl>   <int>
#>  1  6.88   NaN   NaN   NaN  13.8   NaN    -19.6     -85.4   215   NaN   32012
#>  2  7.01   NaN   NaN   NaN  13.8   NaN    -19.6     -85.4   253   NaN   32012
#>  3  6.8    NaN   NaN   NaN  11.4   NaN    -19.6     -85.4   202   NaN   32012
#>  4  7.31   NaN   NaN   NaN  11.4   NaN    -19.6     -85.4   200   NaN   32012
#>  5  7.32   NaN   NaN   NaN  10.8   NaN    -19.6     -85.4   190   NaN   32012
#>  6  7.09   NaN   NaN   NaN  11.4   NaN    -19.6     -85.4   204   NaN   32012
#>  7  7.68   NaN   NaN   NaN  10.8   NaN    -19.6     -85.4   207   NaN   32012
#>  8  7.07   NaN   NaN   NaN  12.9   NaN    -19.6     -85.4   235   NaN   32012
#>  9  7.09   NaN   NaN   NaN  11.4   NaN    -19.6     -85.4   219   NaN   32012
#> 10  6.94   NaN   NaN   NaN  10     NaN    -19.6     -85.4   201   NaN   32012
#> # ℹ 84,908 more rows
#> # ℹ 9 more variables: tide <dbl>, time <dttm>, vis <dbl>, wd <int>, wspd <dbl>,
#> #   wspu <dbl>, wspv <dbl>, wtmp <dbl>, wvht <dbl>
```

<sup>Created on 2026-01-30 with [reprex v2.1.1](https://reprex.tidyverse.org)</sup>

Now you can work with this data in R for whatever purpose you'd like.

### Get data by geographic region

If you want to simply pull all of the buoys in a specific region you can do this too. ERDDAP™ has a few server side functions that let you narrow your search. For example, lets pull all stations within a region along the Northeast USA seaboard, (around Cape cod, MA) between latitudes [41.6,41.8] and longitudes [-70.5, -69.5]

``` r
erddap_url <- 'https://coastwatch.pfeg.noaa.gov/erddap/'
datasetid <- "cwwcNDBCMet"
data_info <- suppressMessages(rerddap::info(datasetid, url = erddap_url))

variables <- data_info$variables$variable_name

rerddap::tabledap(
  datasetid,
  fields = variables,
  'latitude>=41.6','latitude<=41.8','longitude<=-69.5','longitude>=-70.5'
)
#> <ERDDAP tabledap> cwwcNDBCMet
#>    Path: [C:\Users\ANDREW~1.BEE\AppData\Local\Temp\RtmpSQAjbB\R\rerddap\c9f6726fc0a4f89d39c0c92913fabd90.csv]
#>    Last updated: [2026-01-30 11:00:00.736641]
#>    File size:    [54.57 mb]
#> # A tibble: 501,476 × 20
#>      apd  atmp   bar  dewp   dpd   gst latitude longitude   mwd  ptdy station
#>    <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>    <dbl>     <dbl> <int> <dbl> <chr>  
#>  1   NaN   4.9  998.   NaN   NaN   9.8     41.7     -70.0    NA   NaN CHTM3  
#>  2   NaN   4.9  998.   NaN   NaN  11.9     41.7     -70.0    NA   NaN CHTM3  
#>  3   NaN   4.9  998    NaN   NaN  11.3     41.7     -70.0    NA   NaN CHTM3  
#>  4   NaN   4.9  998    NaN   NaN  10.8     41.7     -70.0    NA   NaN CHTM3  
#>  5   NaN   5    998.   NaN   NaN  10       41.7     -70.0    NA   NaN CHTM3  
#>  6   NaN   5    998.   NaN   NaN  11.4     41.7     -70.0    NA   NaN CHTM3  
#>  7   NaN   5.1  998.   NaN   NaN   9.9     41.7     -70.0    NA   NaN CHTM3  
#>  8   NaN   5    998.   NaN   NaN  11.6     41.7     -70.0    NA   NaN CHTM3  
#>  9   NaN   5    998.   NaN   NaN   8.9     41.7     -70.0    NA   NaN CHTM3  
#> 10   NaN   4.9  998.   NaN   NaN  10.6     41.7     -70.0    NA   NaN CHTM3  
#> # ℹ 501,466 more rows
#> # ℹ 9 more variables: tide <dbl>, time <dttm>, vis <dbl>, wd <int>, wspd <dbl>,
#> #   wspu <dbl>, wspv <dbl>, wtmp <dbl>, wvht <dbl>
```

<sup>Created on 2026-01-30 with [reprex v2.1.1](https://reprex.tidyverse.org)</sup>

From this narrow geographic region there is only one station `CHMT3` which is a buoy off Chatham, MA

## Summary

Getting the data from ERDDAP™ into R requires a little work. You can use a combination of the R package `rerddap` and the ERDDAP™ website to help identify the data you are interested in.

Fortunately for the buoy station data used in the example, there is an R package called [`buoydata`](https://noaa-edab.github.io/buoydata/) available to help with the [identification](https://noaa-edab.github.io/buoydata/articles/buoymap.html) of buoy station availability with tools to pull station data hassle free.

Enjoy exploring the masses of data on ERDDAP™


