---
title: 'Air: Formatting R code'
date: 2025-07-15
permalink: /posts/2025/07/air_formatting/
tags:
  - R
  - GitHub
  - git
---

Have you been thinking that you should probably start using a style guide? No, i don't mean a fashion guru! I mean following a guide to help maintain code formatted similarly across all functions and scripts. Consistent white space, consistent indentations in conditional statements and for loops. Code formatted exactly the same way ALL the time? 

In R, this is pretty easy to do and it involves the use of the tool [`Air`](https://tidyverse.org/blog/2025/02/air/) created by the Posit folks. Described as "an extremely fast R formatter" it will format your code every time you hit save. Best used in conjunction with the [tidyverse style guide](https://style.tidyverse.org/)

As of the time of writing: auto formatting is ONLY applied to `.r`, `.R` files. R chunks inside R-Markdown files ('rmd`) are not formatted.

Note: Air is a [*layout* formatter](https://posit-dev.github.io/air/formatter.html), "ensuring that whitespace, newlines, and other punctuation conform to a set of rules and standards"

### Reformatting existing code

Using this tool you can reformat ALL existing code in a repo. From the terminal simply type the command `air format .`. Or if you'd prefer you can open every R file and hit save. I know which one i'll choose!

### Reformatting all future code

The easiest way to keep the repo formatted consistently is to include an `air.toml` file in the root directory of your repo. The file can remain empty, or you can add specific formatting you'd like in your repo. If left empty `air` will apply defaults settings. This is often a good option. 

The benefit of using this `air.toml` file is that anyone who contributes to your repo will automatically have their contributed code formatted as dictated by the `toml` file, rather than any specific user level settings. A no worry solution for consistent formatting of code. The only caveat is that contributors will need to have air installed too, but having the `air.toml` in the repo and indicating that air formatting is required by using a [pull request template]({{site.baseurl}}/posts/2025/06/github_templates/)), for example, this should be painless.

### Reformatting code using GitHub Actions

If you want to take things to the next level, you can create a GitHub action to run on all pull requests. The action will check for formatting inconsistencies and fail if formatting is required. If this is of interest see the [`air`](https://posit-dev.github.io/air/integration-github-actions.html) documentation


Now all you need to do is worry about is coding styles, a subject we'll talk about at a later date.



