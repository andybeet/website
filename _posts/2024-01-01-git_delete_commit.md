---
title: 'Delete your last commit from GitHub'
date: 2024-01-01
permalink: /posts/2024/01/git_delete/
tags:
  - git
---

We've all done it, you'll be happily commiting and pushing to GitHub from either the command line or, more than likely, your favorite IDE when you panic and realize that you just mistakenly pushed something to GitHub. You start sweating, and frenetically start googling how to reverse this. Well it turns out it is pretty simple.

Lets go over an example. Shown below are the last two commits from one of my GitHub repositories

[git_delete]("images/posts/git_delete.png")

Let's suppose i want to delete the very last commit. You first need to make sure the branch is not "protected". To do this check `Settings` -> `Branches` in the repo.

Then you need to copy the [SHA](https://docs.github.com/en/pull-requests/committing-changes-to-your-project/creating-and-editing-commits/about-commits) (from the figure above: 4492eea) of the last commit.

And finally run the following commands in the terminal in the branch you want to remove the commit from. In this example it is the `main` branch

```
git reset --hard 4492eea
git push --force origin main
```

That's it! Gone!



