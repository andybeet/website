---
title: "buoydata"
excerpt: "R package serving National Data buoy Center data<br/><img src='/images/buoydata-250px.png'>"
collection: portfolio
---

`buoydata` is a n R package that allows a user to easily download and process buoy data hosted by [National Data Buoy Center](https://www.ndbc.noaa.gov/).

`buoydata` allows a user to download multiple years of data and stitch them together in a single data frame.

The package comes with lazily loaded station description data provided which combines many station attributes by which to filter.

Currently the package only downloads Historic data, defined as data from January - December for all years prior to the current year.