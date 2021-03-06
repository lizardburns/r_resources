---
title: "Learning Shiny"
author: "Stephen J. Price"
date: "2021-06-03"
output:
  html_document:
    theme: flatly
    df_print: paged
    toc: true
    toc_float: true
    code_folding: show
    keep_md: true
---

Data camp course: Building web applications with R Shiny - case studies




# Resources:
[Mastering Shiny](https://mastering-shiny.org/)  
[Extensions](https://awesomeopensource.com/project/nanxstats/awesome-shiny-extensions)
[Developer conference](https://github.com/rstudio/ShinyDeveloperConference/tree/master/Modules)

1. [Application layout guide](https://shiny.rstudio.com/articles/layout-guide.html)
  - add [text/input to navBarPage](https://github.com/daattali/advanced-shiny/tree/master/navbar-add-text)
  - offer ability to toggle
    + simple [toggle](https://github.com/daattali/advanced-shiny/tree/master/simple-toggle)
    + or see below
2. Styling
    [Free themes for Bootstrap](https://bootswatch.com/)
    [Shiny Themes](http://rstudio.github.io/shinythemes/)
3. Inputs
  - select input
    + with [groupings](https://github.com/daattali/advanced-shiny/tree/master/dropdown-groups)
4. Bespoke error messages with [validate](https://shiny.rstudio.com/articles/validation.html)
5. [Shiny options](https://shiny.rstudio.com/reference/shiny/1.0.3/shiny-options.html)

[javascript](https://book.javascript-for-r.com/)

# Tips
[Extensive list of tips and tricks](https://github.com/daattali/advanced-shiny)

# Example sites
[Oyster](https://jleach.shinyapps.io/oyster/)

# Shiny app components:

1. ui (e.g. sidebarPanel & mainPanel)
2. server

# Shiny fundamentals
## reactivity
- automatic updating of objects whenever one of their dependents changes
- use e.g. `observe()` to [look inside a reactive object](https://shiny.rstudio.com/reference/shiny/1.0.3/observeEvent.html)
- use e.g. `reactive()` to create reactive variables
  - these reactive variables must share some structural features with functions since you follow them with parentheses when you want to refer to them, i.e. x()
- [dealing with double refresh in nearPoints](https://code-examples.net/en/q/1d8e61c)
  

```r
ui <- fluidPage(
  sidebarPanel(
    h1("Options:")
  ),
  mainPanel(

  )
)

server <-
  
shinyApp(ui = ui, server = server)
```

## [Input types](https://shiny.rstudio.com/tutorial/written-tutorial/lesson3/)
- [action buttons](https://shiny.rstudio.com/articles/action-buttons.html)


```r
# Define UI for the application
ui <- fluidPage(
  sidebarLayout(
    sidebarPanel(
      textInput("title", "Title", "GDP vs life exp"),
      textAreaInput(),
      fileInput("file", "File"), # use readLines(input$file$datapath) to get at this file in the server code
      actionButton(),
      numericInput("size", "Point size", 1, 1),
      checkboxInput("fit", "Add line of best fit", FALSE),
      # Add radio buttons for colour
      radioButtons("color", "Point color",
                   choices=c("blue","red","green", "black"))
      # or use colour picker
      colourInput("color", "Point colour", value = "blue"),
      # Add a continent dropdown selector
      selectInput("continents", "Continents",
                  choices = levels(gapminder$continent),
                  multiple = TRUE,
                  selected = "Europe")
      sliderInput("years", "Years", 
                  min(gapminder$year), max(gapminder$year),
                  value=c(1977,2002))),
    mainPanel(
      plotOutput("plot")
    )
  )
)

# Define the server logic
server <- function(input, output) {}

# Run the application
shinyApp(ui = ui, server = server)
```

## output arguments

- eg, width and height


```r
plotOutput("plot", width=600, height=600)
```

## interactive plots
[plotly book](https://plotly-r.com/linking-views-with-shiny.html)


```r
# ui main panel:
plotlyOutput("plot")
# server:
output$plot <- renderPlotly({
  ggplotly({
    data <- subset(gapminder,
                   continent %in% input$continents &
                     year >= input$years[1] & year <= input$years[2])
    
    p <- ggplot(data, aes(gdpPercap, lifeExp)) +
      geom_point(size = input$size, col = input$color) +
      scale_x_log10() +
      ggtitle(input$title)
    
    if (input$fit) {
      p <- p + geom_smooth(method = "lm")
    }
    p
  })
})
```

## download data

```r
# ui
downloadButton(outputId = "download_data", label = "Download")
# server
output$download_data <- downloadHandler(
  # The downloaded file is named "gapminder_data.csv"
  filename = "gapminder_data.csv",
  content = function(file) {
    # The code for filtering the data is copied from the
    # renderTable() function
    data <- gapminder
    data <- subset(
      data,
      lifeExp >= input$life[1] & lifeExp <= input$life[2]
    )
    if (input$continent != "All") {
      data <- subset(
        data,
        continent == input$continent
      )
    }
    
    # Write the filtered data into a CSV file
    write.csv(data, file, row.names = FALSE)
  }
)
```

## Conditional panels
Be careful to use the correct syntax, especially in the condition statement where you need to use . not $!  


```r
ui <- fluidPage(
  h1("Word Cloud"),
  sidebarLayout(
    sidebarPanel(
      radioButtons(
        inputId = "source",
        label = "Word source",
        choices = c(
          "Art of War" = "book",
          "Use your own words" = "own",
          "Upload a file" = "file"
        )
      ),
      conditionalPanel(
        condition = "input.source == 'own'",
        textAreaInput("text", "Enter text", rows = 7)
      ),
      # Wrap the file input in a conditional panel
      conditionalPanel(
        # The condition should be that the user selects
        # "file" from the radio buttons
        condition = "input.source == 'file'",
        fileInput("file", "Select a file")
      ),
      numericInput("num", "Maximum number of words",
                   value = 100, min = 5),
      colourInput("col", "Background color", value = "white")
    ),
    mainPanel(
      wordcloud2Output("cloud")
    )
  )
)

server <- function(input, output) {
  data_source <- reactive({
    if (input$source == "book") {
      data <- artofwar
    } else if (input$source == "own") {
      data <- input$text
    } else if (input$source == "file") {
      data <- input_file()
    }
    return(data)
  })
  
  input_file <- reactive({
    if (is.null(input$file)) {
      return("")
    }
    readLines(input$file$datapath)
  })
  
  output$cloud <- renderWordcloud2({
    create_wordcloud(data_source(), num_words = input$num,
                        background = input$col)
  })
}

shinyApp(ui = ui, server = server)
```

## Speed - use reactive objects
It's important to [isolate each input in it's own reactive](https://shiny.rstudio.com/tutorial/written-tutorial/lesson6/) in order to avoid unneccessary recalculation which will slow down your app!  

# Functionality and appearance
## Date range input
eg. [here](http://shiny.rstudio-staging.com/gallery/date-and-date-range.html)

```r
dateRangeInput('dateRange',
      label = 'Date range input: yyyy-mm-dd',
      start = lubridate::today() - lubridate::days(2), end = lubridate::today()
    )
```

## Tables
https://appsilon.com/forget-about-excel-use-r-shiny-packages-instead/

### [rhandsontable](https://jrowen.github.io/rhandsontable/)

### DT::datatable
[Excellent resource](https://rstudio.github.io/DT/shiny.html)
[datatable options](https://datatables.net/reference/api/buttons.exportInfo)
[datatable buttons](https://rstudio.github.io/DT/003-tabletools-buttons.html)


```r
DT::dataTableOutput("table")
DT::renderDataTable() # gives search bar, control over how many rows are displayed, rnotebook style scrolling
```

#### [Selecting rows](https://yihui.shinyapps.io/DT-rows/)

#### Editable DT::datatables
https://yihui.shinyapps.io/DT-edit/
https://github.com/rstudio/DT/pull/480
https://community.rstudio.com/t/editing-a-reactive-dt-table-that-remembers-the-filtering-context-without-page-flickering/28826

#### Datatable filters
[Searching](https://rstudio.github.io/DT/007-search.html)
  - with client-side processing, you can just enter multiple terms into search bars separated by a space and it uses them all e.g. creates OR logic
  - with server-side processing [this doesn't work](https://stackoverflow.com/questions/36071460/searching-multiple-columns-of-datatable-within-shiny/48129945#48129945) so need to use regex and '|' (not space) to filter on multiple terms: use 'options = list(search = list(regex = TRUE))'.

#### Download all records
https://github.com/rstudio/DT/issues/267

## Points interaction
[Selecting points](https://shiny.rstudio.com/gallery/plot-interaction-selecting-points.html)

- brushedPoints() enables you to draw a box around points
- nearPoints() just lets you click individual points

[also here](https://shiny.rstudio.com/articles/selecting-rows-of-data.html)  

[Bar/box plots](https://shiny.rstudio.com/articles/plot-interaction-advanced.html)  
  + [also here](https://stackoverflow.com/questions/41654801/r-shiny-plot-click-with-geom-bar-and-facets)

[Fade out the points after clicking them](https://gallery.shinyapps.io/106-plot-interaction-exclude/)  

[Zooming in on standard ggplot](https://shiny.rstudio.com/gallery/plot-interaction-zoom.html)  



```r
mtcars2 <- mtcars[, c("mpg", "cyl", "disp", "hp", "wt", "am", "gear")]

ui <- fluidPage(
  fluidRow(
    column(width = 4,
      plotOutput("plot1", height = 300,
        # Equivalent to: click = clickOpts(id = "plot_click")
        click = "plot1_click",
        brush = brushOpts(
          id = "plot1_brush"
        )
      )
    )
  ),
  fluidRow(
    column(width = 6,
      h4("Points near click"),
      verbatimTextOutput("click_info")
    ),
    column(width = 6,
      h4("Brushed points"),
      verbatimTextOutput("brush_info")
    )
  )
)

server <- function(input, output) {
  output$plot1 <- renderPlot({
    ggplot(mtcars2, aes(wt, mpg)) + geom_point()
  })

  output$click_info <- renderPrint({
    # Because it's a ggplot2, we don't need to supply xvar or yvar; if this
    # were a base graphics plot, we'd need those.
    nearPoints(mtcars2, input$plot1_click, addDist = TRUE)
  })

  output$brush_info <- renderPrint({
    brushedPoints(mtcars2, input$plot1_brush)
  })
}

shinyApp(ui, server)
```

## includeMarkdown()
Good for help pages, etc.

## tooltips, pop-ups, popovers, etc (provide user guidance?)
[man page](https://www.rdocumentation.org/packages/shinyBS/versions/0.61/topics/Tooltips_and_Popovers)
https://ijlyttle.github.io/bsplus/articles/modal.html
https://ijlyttle.shinyapps.io/tooltip_popover_modal/


```r
tags$div(title="Click here to slide through years",
    sliderInput("slider_year", "YEAR:", 
                min = 2001, max = 2011, value = 2009, 
                format="####", locale="us"
    )
)
```

## tabs
[navBar](https://shiny.rstudio.com/gallery/navbar-example.html)


```r
tabsetPanel(
    # Create an "Inputs" tab
    tabPanel(
        title = "Inputs",
        sliderInput(inputId = "life", label = "Life expectancy",
                    min = 0, max = 120,
                    value = c(30, 50)),
        selectInput("continent", "Continent",
                    choices = c("All", levels(gapminder$continent))),
        downloadButton("download_data")
    ),
    # Create a "Plot" tab
    tabPanel(
        title = "Plot",
        plotOutput("plot")
    ),
    # Create "Table" tab
    tabPanel(
        title = "Table",
        DT::dataTableOutput("table")
    )
)
```

#### link to other tabs
- expand explainer at top (except help) by including link to help tab in each? - [updateTabsetPanel](https://stackoverflow.com/questions/34315485/linking-to-a-tab-or-panel-of-a-shiny-app)

```r
ui <- fluidPage(

   titlePanel("Old Faithful Geyser Data"),

   tabsetPanel(
     tabPanel("Tab 1", h1("First tab") ),
     tabPanel(
       "Tab2",
       sidebarLayout(
         sidebarPanel(
           width = 3,
           sliderInput(
             "bins",
             "Number of bins:",
             min = 1,
             max = 50,
             value = 30
             )
           ),
         mainPanel(
           plotOutput("distPlot")
           )
         )
       )
     )
   )

server <- function(input, output) {

   output$distPlot <- renderPlot({
      x    <- faithful[, 2] 
      bins <- seq(min(x), max(x), length.out = input$bins + 1)

      hist(x, breaks = bins, col = 'darkgray', border = 'white')
   })
}

shinyApp(ui = ui, server = server)
```

## Toggle sidebar

```r
library(shiny)
library(shinyjs)

ui <- fluidPage(
  useShinyjs(),
  navbarPage(
    "",
    tabPanel(
      "tab",
      div(
        id ="Sidebar",
        sidebarPanel(
          
        )
        ),
      mainPanel(
        actionButton("toggleSidebar", "Toggle sidebar")
        )
      )
    )
  )

server <-function(input, output, session) {
  observeEvent(input$toggleSidebar, {
    shinyjs::toggle(id = "Sidebar")
  })
}

shinyApp(ui, server)
```


```r
includeCSS() # imports a css file where css is stored separately

# otherwise make a variable in shiny app script
my_css <- "
#download_data {
  /* Change the background color of the download button to orange. */
  background: orange;

  /* Change the text size to 20 pixels. */
  font-size: 20px;
}

#table {
  /* Change the text color of the table to red. */
  color: red;
}
"

# then include the following call within the ui specification
tags$style(css_variable_name)
```

## Using client information
https://shiny.rstudio.com/articles/client-data.html
https://stackoverflow.com/questions/54444742/how-to-get-username-logged-into-shiny-app-hosted-on-shinyapps-io
https://stackoverflow.com/questions/40904966/determine-user-credentials-in-shinyapps-io-not-pro
https://stackoverflow.com/questions/55284925/get-authentication-username-from-shinyapp-io
https://docs.rstudio.com/connect/user/shiny.html

# Debugging
[conference talk](https://rstudio.com/resources/shiny-dev-con/debugging-techniques/)

# Modules
[Intro](https://shiny.rstudio.com/articles/modules.html)
Conference - [talk](https://rstudio.com/resources/shiny-dev-con/shiny-modules/) and [materials](https://github.com/rstudio/ShinyDeveloperConference/tree/master/Modules).
[With the R6 class](http://www.chenghaozhu.net/posts/en/2019-03-25/)

# Docker
- [deploy with docker](https://www.r-bloggers.com/2021/05/host-shiny-apps-with-docker/)
- [Dockerized Shiny Apps with Dependencies](https://hosting.analythium.io/dockerized-shiny-apps-with-dependencies/)

# Multi-page shiny apps
https://colinfay.me/brochure-r-package/
