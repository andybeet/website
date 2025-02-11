---
title: 'Include DESCRIPTION file in all R projects'
date: 2024-02-15
permalink: /posts/2024/02/description/
tags:
  - R
---

The `DESCRIPTION` file is a necessary staple for all R packages. Among other things (which we won't get into, since this isn't a post about creating R packages) it is the location for you to list all dependencies of your package. So when your package is installed the user also gets all the dependent packages installed too! Nice and tidy!

However, it isn't a bad idea (for the sake of reproducibility) to have a `DESCRIPTION` file in all of your R projects, regardless of whether they are a package or not. This is especially important if you are hosting this project on GitHub and want others to collaborate. Here's why ...

Since you have an R project, and not an R package, any user that clones your GitHub repo wont know which packages your code is dependent on. This could get frustrating if they have to iteratively install a package, run the code, see it fail, install another package ... etc.

If you have a `DESCRIPTION` file and have your dependencies listed within, then anyone can simply use a function from the [`devtools'](https://devtools.r-lib.org/) package to install all dependencies

Here's how ...

```r
devtools::install_deps()
```

This practice is also good for future you. Maybe you get a new machine or install a new version of R. This will help ease the pain of getting you back up to speed