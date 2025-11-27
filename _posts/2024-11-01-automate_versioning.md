---
title: 'Automate versioning for R data packages'
date: 2024-11-01
permalink: /posts/2024/11/rpackage_versioning/
tags:
  - GitHub
  - workflows
  - R
---

Do you find yourself downloading data from a server over and over again to make sure you have the latest data?
Then struggle to work out what's changed from the last time you downloaded it?

If so, why not why not create an R package to version and document the data changes. That way you'll always be able to go back to an older version and you'll be able to track what has changed through time.

You can do all of this using a GitHub action. Within the workflow, the steps you'll need are (not limited to):

* A function to connect to the server or API and download the data on a defined schedule
* A function to compare the new data pull with the current data in the package
* An optional step to email yourself a report explaining the differences, if any (using a parameterized Rmd)
* An optional step to update the README.md file to inform the user of the date of the most recent data pull
* A function to update the version number in the DESCRIPTION file
* A function to document the data changes in the NEWS.md file
* Commit all changes to the repo
* Create a new GitHub release using the notes added to the NEWS.md

This seems like a lot, and it might take a while to get things set up and working correctly, but the benefits are huge!

## Example: Stocksmart package

As a real example, consider the R data package [stocksmart](https://noaa-edab.github.io/stocksmart/). This is an R data package that serves up data from all federally managed fish stocks in the USA.
Our group relies on this data to update annual ecosystem reports. Having the data automatically pulled, versioned, and documented saves us a lot of time.

The specifics of this [workflow](https://github.com/NOAA-EDAB/stocksmart/blob/main/.github/workflows/stocksmartapi.yml) are:

* A [cron job]({{site.baseurl}}/posts/2024/06/cronjobs/) job to update this data set running on a schedule every Wednesday at noon EST.
* Following [semantic versioning](https://semver.org/), these data changes are considered patch fixes and hence the version number will increment by 0.0.1 each time the data is updated.
* A report is emailed to me after the workflow has finished running outlining the changes seen in the data. This is achieved using the [Send mail GitHub action](https://github.com/dawidd6/action-send-mail) 
* The readme.Rmd and readme.md are updated to add the date of the most recent pull
* The [NEWS.md](https://noaa-edab.github.io/stocksmart/news/index.html) and the DESCRIPTION file are updated to outline changes
* All files are committed to the repo
* If changes are found a new release is created using the [usethis](https://usethis.r-lib.org/) package. specifically the `use_github_release` function.

You will need to add [secrets](https://docs.github.com/en/actions/how-tos/write-workflows/choose-what-workflows-do/use-secrets) to your repo to allow for the email action and the GitHub release function to behave as expected. Once set up this workflow is a maintenance light way to always have up-to date data to work with in R, not only for you, but for the R community at large!

Happy coding!
