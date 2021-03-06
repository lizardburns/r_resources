---
title: "Git and GitHub: version control and collaboration"
author: "Stephen J. Price"
date: "2021-06-03"
output:
  html_document:
    theme: flatly
    df_print: paged
    toc: true
    toc_float: true
    code_folding: hide
    keep_md: true
---

# Accounts/workflow/etc
- [Types of account](https://docs.github.com/en/github/getting-started-with-github/types-of-github-accounts)
- what to do about individuals? i.e. create a second account with ofqual email so one for work and one for personal
- [pricing](https://github.com/pricing)
  - the free organisation account seems perfectly adequate for us to get started:
    + unlimited private repos
    + unlimited collaborators
    - no code owners for private repos
    - no enforced review for private repos so relies on users to do the right thing

# Installation of git and setting up version control for a project
- [Get started](https://support.rstudio.com/hc/en-us/articles/200532077-Version-Control-with-Git-and-SVN)
  + [with Hadley](http://r-pkgs.had.co.nz/git.html)
- [Good advice on installation](https://happygitwithr.com/install-git.html) https://happygitwithr.com/install-intro.html
- [getting ready to commit](https://happygitwithr.com/hello-git.html)

# Connect to GitHub for push/pulls
- You'll need to create a GitHub account if you don't have one
- follow the instructions for setting up [here](https://happygitwithr.com/push-pull-github.html#push-pull-github) and cloning a repo [here](https://happygitwithr.com/rstudio-git-github.html#rstudio-git-github)
- check that you can push from your local repo to github
- [authentication](https://happygitwithr.com/credential-caching.html#credential-caching): you probably don't need to do anything after first authentication but just in case!
- how you operate normally will depend on whether a local RStudio project or a github repo is your starting point - [see here](https://happygitwithr.com/usage-intro.html#usage-intro)

# GitHub and markdown
[Essentials](https://happygitwithr.com/rmd-test-drive.html#rmd-test-drive)

# Forking
- [intro](https://happygitwithr.com/fork-and-clone.html#fork-and-clone)
- [use this](https://usethis.r-lib.org/) package
- after cloning, create a new branch from *master* to work on
  + don't commit to master (this will make it much easier to pull changes from the owner's repo)
- [sync upstream changes to the owner's repo to your cloned repo](https://happygitwithr.com/upstream-changes.html#upstream-changes)
  + https://happygitwithr.com/upstream-changes.html#upstream-changes
- [merging and conflicts](http://r-pkgs.had.co.nz/git.html#git-pull)

# Commit best practise
It's good to link commits to issues since this automatically gives the "why?" and the "how?" is already in the diff.  

- [see here](http://r-pkgs.had.co.nz/git.html#commit-best-practices)


```r
# install.packages("usethis")
```

# Fixes
- Resolves issues with pushing to GitHub due to not having set up git properly and use of personal email with commits:
  + https://stackoverflow.com/questions/43378060/meaning-of-the-github-message-push-declined-due-to-email-privacy-restrictions
  + https://stackoverflow.com/questions/4981126/how-to-amend-several-commits-in-git-to-change-author/25815116#25815116
- [Transferring repo ownership](https://docs.github.com/en/github/administering-a-repository/transferring-a-repository)

# Resources:
- https://nicercode.github.io/git/
- [introductory course](https://swcarpentry.github.io/git-novice/)
- [quick intro](https://www.freecodecamp.org/news/learn-the-basics-of-git-in-under-10-minutes-da548267cc91/)
- [chapter from R packages book](https://r-pkgs.org/git.html)
- [happy git book](https://happygitwithr.com/)
- https://rfortherestofus.com/2021/02/how-to-use-git-github-with-r/
- [gitdown](https://github.com/Thinkr-open/gitdown) - build a bookdown report of commit messages arranged according to a pattern
