---
title: 'Add release tags in git'
date: 2024-08-01
permalink: /posts/2024/08/releasetags/
tags:
  - git
  - GitHub
---

Scenario: You have a repository in which you've been using for a while. You use it for your scientific work and have published papers based on it.
However, you never created releases of the repo corresponding to each publication and now you have no way of reproducing the work used in your publications.
What can you do?

Well, you can add release tags to any "old" commit, providing you can identify the commits! 
Now this isn't a recommended practice or a substitute for not following best practices, but in a pinch it can help out "the younger, inexperienced you of years gone by".

Turns out it is pretty simple too!

First, identify the commit hash. Second, decide on the tagname you want to assign to this commit. Then, run the `git tag` command.

For example, if your hash is `1e4b567712d785bb972665a2edd9401a17d9875d` and you want to tag this with the name, `v1.3.1` then you'd run the following lines of code in the terminal

```
git tag v1.3.1 1e4b567712d785bb972665a2edd9401a17d9875d
git push origin v1.3.1
```

If you want to annotate the release, and store the taggers name, date, time, along with a message you can use the following:

```
git tag -a v1.3.1 1e4b567712d785bb972665a2edd9401a17d9875d -m "Version 1.3.1 Release"
git push origin v1.3.1
```

Repeat this as many times as you'd like. 

In GitHub you should be able to now create a release using this tag!

Make sure you add good release notes in the description field so you know why you tagged this particular point in time as a release point!



