---
title: "buoydata"
excerpt: "R package serving National Data Buoy Center (NDBC) data from all buoys around the world<br/><img src='/images/buoydata-250px.png'>"
collection: portfolio
---

`buoydata` is an R package that allows a user to easily download and process buoy data hosted by [National Data Buoy Center](https://www.ndbc.noaa.gov/).

`buoydata` allows a user to download multiple years of data and stitch them together in a single data frame.

The package comes with lazily loaded station description data provided which combines many station attributes by which to filter.

Currently the package only downloads Historic data, defined as data from January - December for all years prior to the current year.

* Github: View the [buoydata](https://github.com/NOAA-EDAB/buoydata?tab=readme-ov-file#buoydata-) R package