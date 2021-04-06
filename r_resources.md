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

# Rmarkdown
- [R Markdown: The Definitive Guide](https://bookdown.org/yihui/rmarkdown/)
- [Reference Guide/cheat sheet](https://rstudio.com/wp-content/uploads/2015/03/rmarkdown-reference.pdf)
- [RMarkdown for Scientists](https://rmd4sci.njtierney.com/)
- [Using bookdown features](https://bookdown.org/yihui/bookdown/a-single-document.html)
- [font awesome](https://github.com/rstudio/fontawesome)

# Dataviz
- [ragg graphics device](https://ragg.r-lib.org/)

## ggplot2
- [ggplot2 book](https://ggplot2-book.org/index.html)
- [A useful range of charts](http://r-statistics.co/Top50-Ggplot2-Visualizations-MasterList-R-Code.html)
- [ggplot2 extensions](https://exts.ggplot2.tidyverse.org/gallery/)
- [BBC cookbook](https://bbc.github.io/rcookbook/) (also see [repo](https://github.com/bbc/bbplot/tree/master/R))
- [aesthetics specifications](https://ggplot2.tidyverse.org/articles/ggplot2-specs.html) ggplot2, C:/Users/stephen.price/Documents/R/R-4.0.4/library/ggplot2, ggplot2-specs, ggplot2-specs.Rmd, Aesthetic specifications, ggplot2-specs.R, ggplot2-specs.html
- [theme components](https://ggplot2.tidyverse.org/reference/theme.html)
- [Extending ggplot2](https://ggplot2.tidyverse.org/articles/extending-ggplot2.html)
- [colour schemes](https://data-se.netlify.app/2018/12/12/changing-the-default-color-scheme-in-ggplot2/)

### styling labs
  + https://www.ericekholm.com/posts/2021-03-24-improving-ggplots-with-text-color/
  + e.g. subtitle = "Test results from a single year summarised as either a pass or <span style = 'color:#7C272E;'>fail</span><br>"

## other packages
- [cowplot](https://wilkelab.org/cowplot/index.html)
- [ggforce](https://rviews.rstudio.com/2019/09/19/intro-to-ggforce/) also looks useful (but not perfect!) for zooming in on areas of a plot in a second panel.

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
- [Part-to-whole viz](https://digitalblog.ons.gov.uk/2017/08/17/stacking-the-bars-in-your-favour/) post-pie 😢

# Pipelines
- [targets](https://books.ropensci.org/targets/walkthrough.html)

# Google sheets
- https://datascienceplus.com/how-to-use-googlesheets-to-connect-r-to-google-sheets/