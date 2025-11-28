---
title: 'Versioning R packages'
date: 2025-01-15
permalink: /posts/2025/01/rpackage_versioning/
tags:
  - R
  - workflows
  - GitHub
---

You've reached the point in which you want to officially start to version your R package. How do you go about doing this? When should you version? How often? What versioning scheme should i use? How do i document the changes?

All great questions! :rofl: Lets talk this through ...

So in principle, every time content is pushed to the `main` branch of your R package repository should trigger a versioning/release event. This statement assumes that you are using best practices and have adopted a branching strategy. This earlier post on selecting a [branching strategy]({{site.baseurl}}/posts/2024/07/maindevfeature/) is worth a read if you are unsure of what this means.

Under the branching strategy i like to employ, called feature branching, all new features and bug fixes reside on their own branch until completion. At that point they are pulled into the development branch, often called `dev`, via a pull request. At this point multiple workflows are then ran to check various aspects of the code, typically R-CMD checks. Check out an earlier post on [enhancing your R packages]({{site.baseurl}}/posts/2025/02/rpackage_components/) for more info on this.

If all checks pass, then the development branch is ready to be pulled in to the main branch. If they fail, you'll need to address the issues and fix them. Better you do it now vs a user submitting an issue at a later date. 

When all checks pass you need to update the package version in the DESCRIPTION file and update content in the NEWS.md file to summarize all relevant changes to the package, whether a feature enhancement, a bug fix, or something else.

After you then, merge the pull request from `dev` -> `main` you immediately release the package using the release feature on GitHub. The description of the release should use the information in the NEWS.md file and the release version should match the version number in the DESCRIPTION file. The target commit associated with this release should be the latest commit to the main branch.

So this covers the how, when, and what? But what about the questions relating to how often should a package be released or what versioning scheme should be used?

The versioning scheme that is considered best practice is [semantic versioning](https://semver.org/). An earlier post, [Versioning R packages]({{site.baseurl}}/posts/2024/12/rpackage_components/), goes into more detail on how this relates to R package development.

And with regard to how often you should release, well that depends on a lot of things, how frequently bugs are found and addressed or how quickly you want to add new features. Of course these do not need to be versioned as independent events. You can bundle new features and bug fixes into the same release of your package. You just need to adjust the version number to reflect these changes and document the changes in the NEWS.md file, often termed the changelog.

## Summary

Under this workflow, at any point in time the `main` branch of the repository should represent a working released version of your package. If someone came across your repository and installed your package, it would work just fine! All developmental work, whether new features or bug fixes, should reside on their own branches and only merged into the main branch when fully tested and ready for release! This way the main branch remains "clean" of issues.

Hope this helps!

