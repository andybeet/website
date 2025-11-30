---
title: 'Enhancing an R package'
date: 2025-02-15
permalink: /posts/2025/02/enhancing_rpackages/
tags:
  - R
  - workflows
  - GitHub
  - versioning
---

So you want to create an R package? Well there are a ton of resources available to help with this, the best being the official book from the Posit folks, [R Packages (2e)](https://r-pkgs.org/). However, after the initial set up, organization, and implementation of the package there are several other related components you should think about including to make it function, look, and feel polished!

You can see ALL of the components mentioned in this post implemented in the R package [`stocksmart`](https://noaa-edab.github.io/stocksmart/)

## We'll start with some cosmetic enhancemnents

### Add a Hex

You've seen these. Many site have these cool looking hexes, the Posit team has one for every R package in the [tidyverse](https://tidyverse.org/)! Good news is that they are easy to create and implement.

The package [hexSticker](https://github.com/GuangchuangYu/hexSticker) is a good start to help create a hex! Simply save your design as `logo.png`, add it to your `man/figures` folder, and link to it from your `README.md` with an image tag defined after the name of your package. Something like this:

```
# package_name <img src="man/figures/logo.png" align="right" width="120" />
```

### Create a pkgdown site

If you'd like others to use your package, then having a nice looking website with documentation is essential. A step made very easy using a combination of the [`pkgdown`](https://pkgdown.r-lib.org/) and the [`usethis`](https://usethis.r-lib.org/) packages.

* `pkgdown` will create the website locally from a single function call, `build_site()`. The default layout is pretty good right out of the box, but you can customize a little if you'd like.
* `usethis` will create a GitHub action (with the functions `use_github_action()` or `use_pkgdown_github_pages()`) to redeploy your website everytime you make changes to the code or documentation

Not only will a website make your package more user friendly, it will highlight your documentation and show you where you need to focus more attention. A previous post on [GitHub actions]({{site.baseurl}}/posts/2024/05/githubactions1/) for R packages explain this in more detail.

### Add user guides

Adding a helpful user guide or several articles to introduce a user to your package can enhance the adoption of your package by others. They are also pretty easy to create! If you know `Rmarkdown` or `quarto` then it's a breeze. Just save your `rmd` or `qmd` in the `vignettes` folder of your package and `pkgdown` will take care of integrating them to your site.

### Custom issue templates

Now i'm sure your package is awesome!! But some users may want some additional features that you didn't anticipate, or maybe they found a bug in your code! I know, very unlikely right? :rofl: The standard issue templates provided by GitHub work just fine as a reporting tool, but to avoid a lot of back and forth conversation with a user of you package you can [customize templates](https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests/configuring-issue-templates-for-your-repository) to ask for exactly the information you need from a user to either troubleshoot a bug or add a feature.

These custom templates are written in [YAML](https://learnxinyminutes.com/yaml/), formatted ASCII text. They are saved in the folder `.github/ISSUE_TEMPLATE`

### Contibuting guidelines and code of coduct

Adding a guide with instructions on how users can contribute to your package is worthwhile if you want to avoid potential future headaches! Although this does depend on whether people actually read the guidelines, which often they don't! This then begs the question, why bother? Well the answer is, you can point people to the document when needed without having to waste time explaining the process each time. The same applies to a [code of conduct](https://www.jessesquires.com/blog/2020/01/24/github-default-community-health-files/) document, outlining what you expect from contributors regarding behaviour and the type of environment you want to foster.

Best practices often suggest to add these two documents as markdown, `.md`, files in the root of your repository with names `CODE_OF_CONDUCT.md` and `CONTRIBUTING.md`. A great template, to get you started, can be found at [@jessesquires](https://github.com/jessesquires/.github).

Oh, once again `pkgdown` will take care of integrating them into your site automatically!

## Enhancements to make your package more robust

### R-CMD checks: Continuous integration

One of the most painful debugging experiences you might encounter is when someone informs you that your package wont install on their OS! What do you do? You work on a Windows machine and a MacOS user just informed you they are having issues installing your package! Well the answer is to use [GitHub actions]({{site.baseurl}}/posts/2024/05/githubactions1/). Like the `pkgdown` example above, you can create, very easily, a workflow that will, for every pushed commit or pull request, run a series of checks (analogous to those required when submitting to CRAN) to inform you, amongst other things, if your package can be installed without issues on a variety of operating systems!

Again, the `usethis` package has a function, `use_github_action("check-standard")`, that will create the appropriate workflow YAML for you. This workflow is analogous to the `devtools::check()` function you can run locally which checks your package using "all known best practices"

### Unit tests

Probably one of the least utilized practices in package deployment! Not because unit tests are hard to implement but because it can be hard to think about what kinds of test you need/should implement.

Well, the Posit folks help with the implementation part of unit testing with the [`testthat`](https://testthat.r-lib.org/) package. This package has tools to aid in file structure set up, and includes many tools to aid in different types of tests. And if you've implemented the R-CMD continuous integration workflow (described above) then all of the tests you create will be run when you push a commit or make a pull request!

These tests are pretty important, if thought about carefully, since they should catch many bugs BEFORE you release your package. And if you add new features or change the some of the functionality, these tests should aid in determining if the expected outputs are reasonable and that you haven't introduced unexpected bugs.

### Releases

Versioning your package should be taken very seriously. Reproducibility is so important in todays age of rapidly changing software. If you don't version your software, your future self and others will find it extremely difficult to reproduce old code. In addition it should be used to communicate changes, like bug fixes or new features to users and other developers.

For details regarding when and how to version your package, see [versioning R packages]({{site.baseurl}}/posts/2025/01/rpackage_versioning/).


## Summary

You can see ALL of the components mentioned in this post implemented in the R package [`stocksmart`](https://noaa-edab.github.io/stocksmart/)

