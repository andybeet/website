---
title: 'Using GitHub templates'
date: 2025-06-15
permalink: /posts/2025/06/github_templates/
tags:
  - GitHub
  - markdown
---

Maintaining multiple GitHub repos is rewarding until you're stuck deciphering a flood of disorganized issues and PRs. Eventually, the lack of standard formatting becomes a major bottleneck for any productive workflow. You'd like to see a consistent format for all issues and pull requests! Templates are what you have been looking for!

Issue and Pull Request (PR) templates act as a structured "handshake" between a project maintainer and a contributor. Without them, communication often becomes a messy back-and-forth of "Which version are you using?" or "What does this code actually do?" etc. 

Here are a few reasons why they are great for collaborative teams:

### Thoughtful submission

Templates act as a checklist that forces contributors to think through their submission.

For Issues: Instead of a report saying "It's broken," a template requires a Minimal Reproducible Example [Reprex]({{site.baseurl}}/posts/2024/01/reprex/), environment details (like your R version or OS), and clear steps to recreate the bug.

For Pull Requests: It ensures the author explains the "Why" behind a change, not just the "What."

### Reduces "Review Fatigue"

Reviewing code is mentally taxing. Templates reduce this "cognitive load" by:

Standardizing Layout: When every PR looks the same, reviewers know exactly where to find the testing instructions, the link to the Jira/GitHub issue, and the "breaking changes" warning.

### Informative for new contributors

For someone new to your project, the "New Issue" button can be intimidating.

Templates provide scaffolding. They tell the user exactly what information is valued, making them feel more confident that their contribution will be accepted.

It serves as a subtle way to enforce Contribution Guidelines without making someone read a 2,000-word CONTRIBUTING.md file first.

### Better historical records

Six months from now, when you're wondering why a specific line of code was changed, a well-filled PR template provides the context that a single-line commit message often misses. It captures the intent and the testing process used at the time.

## How to implement templates

In GitHub, templates can be written in either markdown or yaml files and are saved in standard locations 

* .github/ISSUE_TEMPLATE - for issues templates
* .github/PULL_REQUEST_TEMPLATE - for PR templates

You can list as many templates as you like, for example for issues you could include templates for

* `Bug Reporting`
* `Feature Requests`
* `Data Issues`

To see some examples from across the web 

* Pull request examples from [axolo-co](https://github.com/axolo-co/pull_request_template)
* Pull request examples from [devspace](https://github.com/devspace/awesome-github-templates?tab=readme-ov-file#rocket-templates-for-pull-requests)
* The [stocksmart](https://github.com/NOAA-EDAB/stocksmart) R package has examples of both [issue](https://github.com/NOAA-EDAB/stocksmart/tree/main/.github/ISSUE_TEMPLATE) and [pull request]((https://github.com/NOAA-EDAB/stocksmart/blob/main/.github/PULL_REQUEST_TEMPLATE/pull_request_template.md)) templates

And if you want to go one step further ... you can create repository templates. These can be set up to contain all of the above templates, a CONTRIBUTING file, CODE_OF_CONDUCT, LICENSE, auto formatting etc. and whenever a repo is created all of these files are bundled with the creation! Pretty cool!

In summary, templates are easy to create, human readable, easy to implement, and have been proven to reduce hair loss!

Enjoy!