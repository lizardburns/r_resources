---
title: "R packages"
output:
  html_document:
    theme: flatly
    df_print: paged
    toc: true
    toc_float: true
    code_folding: show
    keep_md: true
---



# Source
These are just notes taken from the [R packages book](https://r-pkgs.org/) by Hadley Wickham and Jennifer Bryan, which seems like the definitive resource for getting started with package dev (NB. only if you're a `tidyverse`/RStudio convert!).  

There's considerable overlap but you might also find this [package development workshop book](https://combine-australia.github.io/r-pkg-dev/) useful. Another great source of guidance is the [tidyverse design guide](https://principles.tidyverse.org/index.html).  

# Naming your package
Hadley says:

- I recommend against using periods in package names because it has confusing connotations (i.e., file extension or S3 method).
- You can also check if a name is already used on CRAN by loading http://cran.r-project.org/web/packages/[PACKAGE_NAME].
- Avoid using both upper and lower case letters: doing so makes the package name hard to type and even harder to remember. For example, I can never remember if itโs Rgtk2 or RGTK2 or RGtk2.
- be evocative!
- use abbreviations
- add an 'r'
- google your name and see if it clashes
- the `available` fn in the `available` package is also useful ๐


```r
# install.packages("available")
available::available("qualvizr")
```

# Setup

```r
library(tidyverse)
# install.packages(c("devtools", "knitr", "roxygen2", "testthat"))
# you'll also want Rtools if you don't already have it :)
# library(installr)
# install.Rtools()
library(devtools)
has_devel()
library(testthat)
```

Anything other than "Your system is ready to build packages!" and there's work to do on preparing your build. If you are having trouble, try again in a brand new session.  

Create a package in RStudio in the same way that you create projects but select package instead.  

Alt + Shift + K brings up the keyboard shortcut quick reference page.  

# What is a package?
5 states:

## Source packages
The development version of a package that lives on your computer. A source package is just a directory with components like R/, DESCRIPTION, and so on.

## Bundled packages
- A bundled package is a package thatโs been compressed into a single file. 
- By convention (from Linux), package bundles in R use the extension .tar.gz.
- While a bundle is not that useful on its own, itโs a useful intermediary between the other states.
- In the rare case that you do need a bundle, call devtools::build() to make it.
- If you decompress a bundle, youโll see it looks almost the same as your source package. 

The main differences between an uncompressed bundle and a source package are:
- Vignettes are built so that you get HTML and PDF output instead of Markdown or LaTeX input.
- Your source package might contain temporary files used to save time during development, like compilation artefacts in src/. These are never found in a bundle.
- Any files listed in .Rbuildignore are not included in the bundle.

.Rbuildignore prevents files in the source package from appearing in the bundled package. It allows you to have additional directories in your source package that will not be included in the package bundle. This is particularly useful when you generate package contents (e.g. data) from other files. Those files should be included in the source package, but only the results need to be distributed. This is particularly important for CRAN packages (where the set of allowed top-level directories is fixed). Each line gives a Perl-compatible regular expression that is matched, without regard to case, against the path to each file (i.e. dir(full.names = TRUE) run from the package root directory) - if the regular expression matches, the file is excluded.  

If you wish to exclude a specific file or directory (the most common use case), you MUST anchor the regular expression. For example, to exclude a directory called notes, use ^notes$. The regular expression notes will match any file name containing notes, e.g. R/notes.R, man/important-notes.R, data/endnotes.Rdata, etc. The safest way to exclude a specific file or directory is to use devtools::use_build_ignore("notes"), which does the escaping for you.  

Hereโs a typical .Rbuildignore file from one of HW's packages:

`^.*\.Rproj$`         # Automatically added by RStudio,
`^\.Rproj\.user$`     # used for temporary files. 
`^README\.Rmd$`       # An Rmarkdown file used to generate README.md
`^cran-comments\.md$` # Comments for CRAN submission
`^NEWS\.md$`          # A news file written in Markdown
`^\.travis\.yml$`     # Used for continuous integration testing with travis

## Binary packages
If you want to distribute your package to an R user who doesnโt have package development tools, youโll need to make a binary package. Like a package bundle, a binary package is a single file. But if you uncompress it, youโll see that the internal structure is rather different from a source package:

- no .R files in the R/ directory - instead there are three files that store the parsed functions in an efficient file format.
- Meta/ directory contains a number of Rds files. These files contain cached metadata about the package, like what topics the help files cover and parsed versions of the DESCRIPTION files. (You can use readRDS() to see exactly whatโs in those files). These files make package loading faster by caching costly computations.
- An html/ directory contains files needed for HTML help.
- any code in the src/ directory there will now be in a libs/ directory that contains the results of compiling 32 bit (i386/) and 64 bit (x64/) code.
- contents of inst/ are moved to the top-level directory.

Binary packages are platform specific.  
You can use devtools::build(binary = TRUE) to make a binary package.  

## Installed packages
An installed package is just a binary package thatโs been decompressed into a package library.  

The tool that powers all package installation is the command line tool R CMD INSTALL - it can install a source, bundle or a binary package. Devtools functions provide wrappers that allow you to access this tool from R rather than from the command line. 

- devtools::install() is effectively a wrapper for R CMD INSTALL. 
- devtools::build() is a wrapper for R CMD build that turns source packages into bundles. 
- devtools::install_github() downloads a source package from GitHub, runs build() to make vignettes, and then uses R CMD INSTALL to do the install. 
- devtools::install_url(), devtools::install_gitorious(), devtools::install_bitbucket() work similarly for packages found elsewhere on the internet.

install.packages() and devtools::install_github() allow you to install a remote package. Both work by downloading and then installing the package. This makes installation very speedy. install.packages() is used to download and install binary packages built by CRAN. install_github() works a little differently - it downloads a source package, builds it and then installs it.  

You can prevent files in the package bundle from being included in the installed package using .Rinstignore. This works the same way as .Rbuildignore, described above. Itโs rarely needed.  

## In memory packages
To use a package, you must load it into memory. To use it without providing the package name (e.g. install() instead of devtools::install()), you need to attach it to the search path. R loads packages automatically when you use them. library() (and the later discussed require()) load, then attach an installed package.  

library() is not useful when youโre developing a package because you have to install the package first. devtools::load_all() and RStudioโs โBuild and reloadโ allow you to skip install and load a source package directly into memory.  

# R/
Ctrl/Cmd + Shift + L is your friend - saves and reloads all your files

# Creating your package
## Set up your local repo
- create a directory
- run this command with the path to your new package directory


```r
create_package("qualvizr/")
```

## Add git version control

```r
use_git()
```

## Add some functions
- start with one function per .R file
- use_r() creates these files automatically in the `R/` dir; just supply a file name
- include only the code that defines the function
- load_all() makes your functions available for use (but they won't show in the global environment like they do if you source them from a file, thereby simulating the process of building, installing, and attaching a package)


```r
use_r("theme_ofqual")
use_r("flip_y_title")
use_r("brand_plot")
use_r("palettes")
use_r("alt_textify")

load_all()
# theme_ofqual()
```

## Check everything is working

```r
check()
```

## Licensing
- [Wickham et al intro](https://r-pkgs.org/license.html)
- [choose a license](https://choosealicense.com/licenses/)
- [basic usethis commands](https://r-pkgs.org/whole-game.html#use_mit_license)
- [National Archives intro to open software licenses](https://www.nationalarchives.gov.uk/information-management/re-using-public-sector-information/uk-government-licensing-framework/open-government-licence/open-software-licences/)
- [open government license](http://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/)
  - [about](https://www.nationalarchives.gov.uk/information-management/re-using-public-sector-information/uk-government-licensing-framework/open-government-licence/)
  - [guidance for users](https://nationalarchives.gov.uk/documents/information-management/ogl-user-guidance.pdf)


```r
use_gpl3_license(name = "Stephen J. Price")
```

## Add documentation
- Open one of your function files in RStudio
- insert the cursor anywhere in the source code
- use `Code > Insert Roxygen skeleton` to get a Roxygen2 skeleton
- edit as needed


```r
document()
# ?theme_ofqual
```

After running `document()` you can now access help files as normal, eg `?theme_ofqual`.  

Let's check our progress:


```r
check()
```

## Install

```r
install()
```

## Unit tests
- [detailed description of workflow](https://r-pkgs.org/tests.html)
- [additional guidance](https://github.com/bradleyboehmke/unit-testing-r) and [here](http://www.johnmyleswhite.com/notebook/2010/08/17/unit-testing-in-r-the-bare-minimum/)
- [exampletestr](https://cran.r-project.org/web/packages/exampletestr/index.html) helps build unit tests within the `testthat` framework (github repo [here](https://github.com/rorynolan/exampletestr)) by providing a shell for tests based around the examples included in your function documentation. It doesn't ensure comprehensive unit testing but will help cover the basics within the confines of the examples you have written.
- eg ggplot2 [scales](https://github.com/tidyverse/ggplot2/blob/master/tests/testthat/test-scale-discrete.R)

Unit testing is [underutilised](https://europepmc.org/backend/ptpmcrender.fcgi?accid=PMC5500893&blobtype=pdf) but is a bit of a no-brainer in terms of cutting down the time spent debugging for projects subject to ongoing development and quality-assurance of your projects (for your own sake and for giving confidence to others).  


```r
use_testthat()
# use_test("theme_ofqual")

# install.packages("exampletestr")
library(exampletestr)
make_tests_shells_file("theme_ofqual")
make_tests_shells_file("brand_plot")
make_tests_shells_file("flip_y_title")
make_tests_shells_file("palettes")
make_tests_shells_file("alt_textify")
```

Add some test code to the new file, eg:

```r
test_that("theme ofqual works", {
  expect_is(theme_ofqual(), "theme")
})
```

And run interactively, by using the "Run tests" button (top-right in script viewer panel) to get a report under the `Build` tab or by running the `test()` function which outputs to console.


```r
test()
```

When you've written some unit tests, test the coverage with the [covr package](https://github.com/r-lib/covr). *The `covr` functions throw an error if the package you are checking is loaded so detach first!*


```r
# install.packages("covr")
library(covr)
# detach(qualvizr)
package_coverage()
```

## Add badges

- [lifecycle](https://lifecycle.r-lib.org/articles/lifecycle.html) badges


```r
use_lifecycle_badge("Maturing")
```

### Code coverage badge
Can be added once project is on github. See tis [blog](https://www.r-bloggers.com/2017/06/how-to-add-code-coverage-codecov-to-your-r-package/) if any problems.  


```r
use_coverage(pkg = ".", type = c("codecov"))
```

## Set dependencies
- amends DESCRIPTION
- the second argument determines the nature of the dependency: must be one of "Imports", "Depends", "Suggests", "Enhances", or "LinkingTo"
- [Wickham et al](https://r-pkgs.org/namespace.html#search-path):
  * "Listing a package in either `Depends` or `Imports` ensures that itโs installed when needed. The main difference is that where `Imports` just loads the package, `Depends` attaches it."
  * "Unless there is a good reason otherwise, you should always list packages in `Imports` not `Depends`."
  * "The only exception is if your package is designed to be used in conjunction with another package."


```r
use_package("ggplot2", "Imports")
use_package("cowplot", "Imports")
use_package("magick", "Imports")
use_package("tibble", "Imports")
use_package("purrr", "Imports")
use_package("grDevices", "Imports")
use_pipe()
use_package("dplyr", "Suggests")
use_package("qpdf", "Suggests")
use_package('janitor', "Suggests")
use_package('openxlsx', "Suggests")
use_package('stringr', "Suggests")
use_package('tidyr', "Suggests")
use_package('scales', "Suggests")
use_package('forcats', "Suggests")
use_package('emo', "Suggests")
use_package('ggforce', "Suggests")
check()
```

### Importing functions
- most of the time you can just use `::` notation to import functions from other packages into your functions (and add the package under Imports section of DESCRIPTION)
- for repeated use, eg piping, it is probably easier to add `@importFrom magrittr %>%` to the function documentation

## Including [data](https://r-pkgs.org/data.html)

```r
?use_data
```

### External `data/`

- Each file in this directory should be a .RData file created by `save()` containing a single object (with the same name as the file). The easiest way to adhere to these rules is to use `usethis::use_data()`.
- `usethis::use_data_raw()` will help you to include scripts used to prepare the data in the `data-raw/` dir of your source package
- data included in package should ideally be already published elsewhere and the prep script should include the download from a public url
  * revisions to the source data will only be captured when you rerun these preparatory scripts, so vignettes and the "Source" section of the data help page should include details which make everything transparent.
  * if the data is only used as part of your examples/vignette and the fetching and processing is straightforward then it may be best to include the code there rather than include the data in your package (removing difficulties posed by revisions but potentially creating issues for deprecation if updates are anticipated to urls)
- where there is no suitable published data, toy data should be created, but in this case the preparatory scripts will necessarily need to sit outside your source package.
- [documenting datasets](https://r-pkgs.org/data.html#documenting-data) is very tidy with Roxygen2
  * don't use the `@export` tag with datasets

### Internal in `R/sysdata.rda`
- ie data that your functions need but should not be available to package users
- you can still use `use_data` to set this up but specify with the `internal` arg, eg `usethis::use_data(x, mtcars, internal = TRUE)`
- the script used to generate it can still go in `data-raw/`
- not exported so documentation is not necessary
- NOTE: `data-raw` is not added to .gitignore by default (as you'd expect) so be careful if you are including anything sensitive in your data prep scripts

## Push to GitHub?
- `use_github()`
- Wickham recommends [GitHub first](https://happygitwithr.com/new-github-first.html) (or [here](https://happygitwithr.com/existing-github-first.html)) but it's not always necessary so you might end up doing [GitHub last](https://happygitwithr.com/existing-github-last.html).

## Create a README ahead of creating a vignette
- `use_readme_rmd()`
- `build_readme()` to render it

```r
use_readme_rmd()
build_readme()
```

## Create a vignette

```r
use_vignette("qualvizr")
use_vignette("dataviz_guide")
```

## Use a logo?
- https://usethis.r-lib.org/reference/use_logo.html  
- hex sticker
  + [package](https://github.com/GuangchuangYu/hexSticker)
  + [case study](https://shixiangwang.github.io/home/en/post/2019-06-20-how-i-create-ucscxenatools-logo/)


```r
# install.packages("hexSticker")
# install.packages("nycflights13")
library(hexSticker)
library(nycflights13)

p <- flights %>% 
  count(carrier) %>% 
  sample_n(6) %>% 
  ggplot(aes(x = reorder(carrier, desc(n)), y = n)) + 
  geom_col(fill = qualvizr::ofqual_cols("agree green")) + 
  theme_void() + 
  theme_transparent()

# brand_plot(p)
sticker(p, package="qualvizr", p_size = 20, s_x = 1, s_y = .85, s_width = 1, s_height = .67,
        h_fill = "#505261",
        filename = "logos/qualvizr.png")

use_logo("logos/qualvizr.png")
```

## Continuous integration?
https://usethis.r-lib.org/reference/github_actions.html  

## Build a pkgdown site?
https://pkgdown.r-lib.org/


```r
# install.packages("pkgdown")
use_pkgdown()
build_site()
```

# Documenting the package itself
- [intro](https://r-pkgs.org/man.html#man-packages)


```r
use_package_doc()
```

# Version (release)
- "A released version number consists of three numbers, <major>.<minor>.<patch>. For version number 1.9.2, 1 is the major number, 9 is the minor number, and 2 is the patch number. Never use versions like 1.0, instead always spell out the three components, 1.0.0"
- "An in-development package has a fourth component: the development version. This should start at 9000. For example, the first version of the package should be 0.0.0.9000. There are two reasons for this recommendation: first, it makes it easy to see if a package is released or in-development, and the use of the fourth place means that youโre not limited to what the next version will be. 0.0.1, 0.1.0 and 1.0.0 are all greater than 0.0.0.9000"
  + Increment the development version, e.g. from 9000 to 9001 if youโve added an important feature that another development package needs to depend on.  

# Troubleshooting
- [โno visible bindingโ](https://r-pkgs.org/package-within.html#echo-a-working-package)
- [side effects](https://r-pkgs.org/package-within.html#package-within-side-effects)
  + [withr](https://r-pkgs.org/r.html#manage-state-with-withr)
