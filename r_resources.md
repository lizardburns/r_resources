---
title: "R resources"
author: "Stephen Price"
date: "06/04/2021"
output:
  html_document:
    theme: flatly
    df_print: paged
    toc: true
    toc_float: true
    code_folding: hide
    keep_md: true
---

# Data science and R itself
- [R for data science](https://r4ds.had.co.nz/)
- [Advanced R](https://adv-r.hadley.nz/)
- [Modern R with the tidyverse](https://b-rodrigues.github.io/modern_R/)
- [Data science in Education](https://datascienceineducation.com/)
- [What they forgot to teach you about R book](https://rstats.wtf/index.html)

## Statistical modelling
- [Influential observations in linear regression](https://jmsallan.netlify.app/blog/influential-observations-in-linear-regression/)
- [Bayesmodels](https://github.com/AlbertoAlmuinha/bayesmodels) - unlock multiple bayesian models in one framework.In addition, it allows you to integrate these models with the Modeltime and the Tidymodels ecosystems.
- [broomExtra](https://github.com/IndrajeetPatil/broomExtra) - provides helper functions that assist in data analysis workflows involving regression analyses.
- [statistical learning with tidymodels](https://emilhvitfeldt.github.io/ISLR-tidymodels-labs/statististical-learning.html)
- [visualising machine learning data](https://shirinsplayground.netlify.app/2021/04/goodbadugly_ml/)
- [mixed effect models with lmer](https://github.com/RoseannaGG/LinearMixedEffectsModels/blob/main/Linear%20Mixed%20Effects%20Models%20-%20R%20Gamlen-Greene.pdf)

# Packages
- [creating data packages](https://grasshoppermouse.github.io/posts/2017-10-18-put-your-data-in-an-r-package/)
- [package documentation](https://thisisnic.github.io/2021/05/18/r-package-documentation-what-makes-a-good-example/)
- [how teams can use internal packages](https://www.rstudio.com/resources/rstudioglobal-2021/organization-how-to-make-internal-r-packages-part-of-your-team/)

# Rmarkdown
- [R Markdown: The Definitive Guide](https://bookdown.org/yihui/rmarkdown/)
- [Reference Guide/cheat sheet](https://rstudio.com/wp-content/uploads/2015/03/rmarkdown-reference.pdf)
- [RMarkdown for Scientists](https://rmd4sci.njtierney.com/)
- [Using bookdown features](https://bookdown.org/yihui/bookdown/a-single-document.html)
- [font awesome](https://github.com/rstudio/fontawesome)
- [remedy plugin](https://thinkr-open.github.io/remedy/) - dropdown options to do markdown formatting

# Shiny
- [deploy with docker](https://www.r-bloggers.com/2021/05/host-shiny-apps-with-docker/)
- [Dockerized Shiny Apps with Dependencies](https://hosting.analythium.io/dockerized-shiny-apps-with-dependencies/)
- [app themes](https://shiny.rstudio.com/app-stories/weather-lookup-bslib.html)
- [javascript](https://book.javascript-for-r.com/)

# Dataviz
- [ragg graphics device](https://ragg.r-lib.org/)
- [dataviz package list](https://github.com/krzjoa/awesome-r-dataviz)

## ggplot2
- [ggplot2 book](https://ggplot2-book.org/index.html) and [course](https://datavizs21.classes.andrewheiss.com/)
- [A useful range of charts](http://r-statistics.co/Top50-Ggplot2-Visualizations-MasterList-R-Code.html)
- [ggplot2 extensions](https://exts.ggplot2.tidyverse.org/gallery/)
- [BBC cookbook](https://bbc.github.io/rcookbook/) (also see [repo](https://github.com/bbc/bbplot/tree/master/R))
- [aesthetics specifications](https://ggplot2.tidyverse.org/articles/ggplot2-specs.html) ggplot2, C:/Users/stephen.price/Documents/R/R-4.0.4/library/ggplot2, ggplot2-specs, ggplot2-specs.Rmd, Aesthetic specifications, ggplot2-specs.R, ggplot2-specs.html
- [theme components](https://ggplot2.tidyverse.org/reference/theme.html)
- [colour schemes](https://data-se.netlify.app/2018/12/12/changing-the-default-color-scheme-in-ggplot2/)

### styling labs
  + install `ggtext`
  + append `theme(plot.title = ggtext::element_markdown())` to plot code
  + wrap the text you want to colour or otherwise style in html using `span`
  + e.g. title = "<span style = 'color:#d4681d;'>The coloured bit of your title</span> will then be highlighted"
  + [check out this blog post](https://www.ericekholm.com/posts/2021-03-24-improving-ggplots-with-text-color/)
  
## other packages/extensions
- [Extending ggplot2](https://ggplot2.tidyverse.org/articles/extending-ggplot2.html)
- [12 ggplot extensions](https://mode.com/blog/r-ggplot-extension-packages/)
- [cowplot](https://wilkelab.org/cowplot/index.html)
- [ggforce](https://rviews.rstudio.com/2019/09/19/intro-to-ggforce/) also looks useful (but not perfect!) for zooming in on areas of a plot in a second panel.
- [upset plots](https://www.r-graph-gallery.com/upset-plot.html)
- [geofaceting](https://github.com/hafen/geofacet) - geofaceting functionality for ggplot2

## general dataviz
- [ONS dataviz blogs](https://digitalblog.ons.gov.uk/category/dataviz/)
- [dataviz book](http://book.visualisingdata.com/)
- [GSS dataviz intro](https://gss.civilservice.gov.uk/policy-store/introduction-to-data-visualisation/)
- [Blog on viz style](https://medium.com/nightingale/style-guidelines-92ebe166addc) and accompanying [database of style guides](https://docs.google.com/spreadsheets/d/1F1gm5QLXh3USC8ZFx_M9TXYxmD-X5JLDD0oJATRTuIE/edit#gid=1679646668)
- [Claus Wilke](https://clauswilke.com/dataviz/)

## Colour scales
- https://blog.datawrapper.de/which-color-scale-to-use-in-data-vis/

## Accessibility
- [accessibility blog](http://www.storytellingwithdata.com/blog/2018/6/26/accessible-data-viz-is-better-data-viz)

## infographics
- [pinterest style site](https://informationisbeautiful.net/)

## other
- [Quiz formats](https://digitalblog.ons.gov.uk/2018/11/20/who-wants-to-be-a-millionaire/)
- [Part-to-whole viz](https://digitalblog.ons.gov.uk/2017/08/17/stacking-the-bars-in-your-favour/) post-pie ????

# Tables
- [Table best practise and a grammar of tables {gt}](https://themockup.blog/posts/2020-09-04-10-table-rules-in-r/)

# Pipelines
- [targets](https://books.ropensci.org/targets/walkthrough.html)

# Testing
- [types of testing](https://www.freecodecamp.org/news/types-of-software-testing/)

# Git
- [gitdown](https://github.com/Thinkr-open/gitdown) - build a bookdown report of commit messages arranged according to a pattern

# Productivity
- [job package](https://github.com/lindeloev/job) - free your RStudio console by running jobs in the background

# Google sheets
- https://datascienceplus.com/how-to-use-googlesheets-to-connect-r-to-google-sheets/

# Time series
- [timetk](https://github.com/business-science/timetk)

# Other fun stuff
- [politicaldata: a package for acquiring and analyzing political data](https://github.com/elliottmorris/politicaldata)
- [rayvista](https://github.com/h-a-graham/rayvista) -  plugin for the fabulous {rayshader} package. It provides a single main function plot_3d_vista which allows the user to create a 3D visualisation of any location on earth.
- [climateR](https://github.com/mikejohnson51/climateR)
