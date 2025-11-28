---
title: 'Semantic versioning'
date: 2024-12-01
permalink: /posts/2024/12/semantic_versioning/
tags:
  - R
  - versioning
---

The official site for [Semantic versioning](https://semver.org/) is available in many languages indicating, in part, how important this subject is across the world for software development. It provides a clear, standardized way to communicate the nature of changes between releases, which helps manage dependencies and predict the impact of updates. By using a version format like MAJOR.MINOR.PATCH, developers indicate whether an update is a backwards-incompatible breaking change (MAJOR), a new backwards-compatible feature (MINOR), or a backwards-compatible bug fix (PATCH).

In the development of R packages it is no different. Let's dive a little deeper with some examples. Following semantic versioning we adopt the format of MAJOR.MINOR.PATCH. For example v3.5.1. The interpretation of these three components can be a little confusing, especially in the context of R package development.

* Changes in the PATCH component refer to small changes, like bug fixes. It is assumed that these changes do NOT break any existing code. They are "backward compatible". Nothing changes in the package except an error has been corrected
* Changes in the MINOR component refer to changes such as new features. These new features integrate with the package, they complement other features already present, and do NOT break existing code.
* Changes in the MAJOR component refer to changes that would break existing code. For example, if a user was using a particular function in your package, and you changed the code in this function that resulted in a different output structure, the users code would break. This would be "backward incompatible"

Suppose a package is currently at version 3.5.1

* If one or more bugs were fixed (PATCH), the next release would be version 3.5.2
* If a new feature was added (MINOR), the next release would be version 3.6.0. (Note the PATCH is reset to zero. It would not be 3.6.1)
* If a new breaking change was added (MAJOR), the next release would be version 4.0.0. (Note, both MINOR and PATCH are reset to zero)
* If several bugs were fixed (PATCH) and a couple of new features (MINOR), the next release would be version 3.6.0. (Note: Default to the higher order change, MINOR)
* If several bugs were fixed (PATCH), a couple of new features added (MINOR), and a restructure of the package (MAJOR), the next release would be version 4.0.0. (Note: Default to the higher order change, MAJOR)

I think you get it! Enough said.