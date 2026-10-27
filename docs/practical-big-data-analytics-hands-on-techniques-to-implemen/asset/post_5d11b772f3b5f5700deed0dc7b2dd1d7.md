
```
findInterval(cms$payment, quantile_values, rightmost.closed=TRUE)

cms[, quantileVal:=factor(interval_values,
labels=c("0-25", "25-50", "50-75", "75-100"))]

rules_cols<-
c("category", "city", "company", "paymentNature", "product", "quantileVal")

cms[, (rules_cols) := lapply(.SD, factor), .SDcols = rules_cols]

cms_factor<-
cms[, .(category, city, company, paymentNature, product, quantileVal)]

rhsVal<- paste0("quantileVal", "=", c("0-25", "25-50", "50-75", "75-100"))

cms_rules<-
apriori(cms_factor, parameter=list(supp=0.001, conf=0.25, target="rules", minlen=3))

cms_rules_dt<- data.table(as(cms_rules, "data.frame"))
cms_rules_dt[, c("LHS", "RHS") := tstrsplit(rules, "=>", fixed=TRUE)]
num_cols<- c("support", "confidence", "lift")
cms_rules_dt[, (num_cols) := lapply(.SD, function(x){round(x,2)}), .SDcols = num_cols]

saveRDS(cms_rules_dt, "cms_rules_dt.rds")
saveRDS(cms_factor, "cms_factor_dt.rds")
```

## Shiny (R Code)

In RStudio, select File | New File | Shiny Web App:

![](img/8ededb1e0b69e85687f6e488d1044b79_315_0.png)

```
fluidRow(
  dataTableOutput("result")
)
),
  title = "Aprior Algorithm"
)

# Define server logic required to draw a histogram
server <- function(input, output, session) {

  PLACEHOLDERLIST2 <- list(
    placeholder = 'Select All',
    onInitialize = I('function() { this.setValue(""); }')
  )

  output$company<- renderUI({
    datasetList<- c("Select
All",as.character(unique(sort(cms_factor_dt$company))))
    selectizeInput("company", "Select Company",
    datasetList, multiple = FALSE,options = PLACEHOLDERLIST2,selected="Select
All")
  })

  output$searchlhs<- renderUI({
    textInput("searchlhs", "Search LHS", placeholder = "Search")
  })

  output$searchrhs<- renderUI({
    textInput("searchrhs", "Search RHS", placeholder = "Search")
  })

  output$support2 <- renderUI({
    sliderInput("support2", label =
    'Support',min=0,max=0.04,value=0.01,step=0.005)
  })

  output$confidence<- renderUI({
    sliderInput("confidence", label = 'Confidence',min=0,max=1,value=0.5)
  })

  output$lift<- renderUI({
    sliderInput("lift", label = 'Lift',min=0,max=10,value=0.8)
  })

  dataInput<- reactive({
    print(input$support2)
    print(input$company)
    print(identical(input$company,""))
  })
```

```
temp <- cms_rules_dt[support > input$support2 & confidence >input$confidence& lift >input$lift]

if(!identical(input$searchlhs,"")){
searchTerm<- paste0("*",input$searchlhs,"*")
    temp <- temp[LHS %like% searchTerm]
}

if(!identical(input$searchrhs,"")){
searchTerm<- paste0("*",input$searchrhs,"*")
    temp <- temp[RHS %like% searchTerm]
}

if(!identical(input$company,"Select All")){
    # print("HERE")
    temp <- temp[grepl(input$company,rules)]
}
    temp[, .(LHS,RHS,support,confidence,lift)]
})

output$downloadMatchingRules<- downloadHandler(
    filename = "Rules.csv",
    content = function(file) {
        write.csv(dataInput(), file, row.names=FALSE)
    }
)

output$result<- renderDataTable({
    z = dataInput()
    if (nrow(z) == 0) {
        z <- data.table("LHS" = '', "RHS"='', "Support"='', "Confidence"='', "Lift" = '')
    }
    setnames(z, c("LHS", "RHS", "Support", "Confidence", "Lift"))
    datatable(z,options = list(scrollX = TRUE))
})

} shinyApp(ui = ui, server = server)
```

The following image shows the code being copied and saved in a file called `app.R`.

![](img/8ededb1e0b69e85687f6e488d1044b79_319_0.png)

## Using custom CSS and fonts for the application

For our application, we will use a custom CSS File. We will also use custom fonts in order to give the application a nice look-and-feel.

You can download the custom CSS File from the software repository for this book.

The CSS, Fonts, and other related files should be stored in a folder called `www` in the directory where you created the R Shiny Application:

![](img/8ededb1e0b69e85687f6e488d1044b79_320_0.png)

## Running the application

If all goes well, you should be now able to run the application by clicking on the **Run App** option on the top of the page, as shown in the following images:

![](img/8ededb1e0b69e85687f6e488d1044b79_320_1.png)

Upon clicking the "Run" button, the user will see a popup window similar to the one shown below. Note that popups should be enabled in the browser for this to function.

![](img/8ededb1e0b69e85687f6e488d1044b79_321_0.png)

The app has multiple controls, such as:

- **Search LHS/RHS**: Enter any test that you want to filter for, in the Left-Hand Side or the Right-Hand Side of the rule.
- **Support**: Indicates the prevalence of the rule in the dataset.
- **Confidence**: Of the rules, how many were exact matches.
- **Lift**: Variable defining the importance of a rule. Numbers above 1 are considered significant.

You can use this app for any other rules file as long as they are processed in a way similar to the one outlined before in the R Script section.

## Summary

Machine learning practitioners are often of the opinion that creating models is easy, but creating a good one is much more difficult. Indeed, not only is creating a good model important, but perhaps more importantly, knowing how to identify a good model is what distinguishes successful versus less successful Machine Learning endeavors.

In this chapter, we read up on some of the deeper theoretical concepts in Machine Learning. Bias, Variance, Regularization, and other common concepts were explained with examples as and where needed. With accompanying R code, we also learnt about some of the common machine learning algorithms such as Random Forest, Support Vector Machines, and others. We concluded with a tutorial on how to create an exhaustive web-based application for Association Rules Mining against CMS OpenPayments data.

In the next chapter, we will read about some of the technologies that are being used in enterprises for both big data as well as machine learning. We will also discuss the merits of cloud computing and how they are influencing the selection of enterprise software and hardware stacks.

We will cover the following topics in this chapter:

- Enterprise data science overview
- Enterprise data mining
- Enterprise AI and machine learning
- Enterprise infrastructure
- Other considerations, such as data strategy, governance, and tool selection
- Amazon Web Services tutorial

## Enterprise data science overview

Data science is a relatively new topic in terms of enterprise IT and analytics. Traditionally, researchers and analysts belonged broadly to one of two categories:

- Highly technical researchers who used complex computing languages and/or hardware for their professional tasks
- Analysts who could use tools such as Excel and BI platforms in order to perform both simple and complex data analysis

Organizations started looking into **Big Data** and, more generally, data science platforms in the late 2000s. It had gained immense momentum by 2013, when solutions such as Hadoop and NoSQL platforms were released. The following table shows the developments in data science:

| Year | Developments |
|---|---|
| 1970s to late 1990s | Widespread use of relational database management systems. Entity relationship model, structured query language (SQL), and other developments eventually led to a rapid expansion of databases in the late 90s. |
| Early 2000s | The anti-climatic, yet expensive, non-event of Y2K, coupled with the collapse of the dot-com bubble led to a period of stagnation. In terms of databases, or more generally, data mining platforms, this meant that companies were less focused on new innovations than they were on keeping the business running. |

| 2005-2010 | The industry slowly recovered, but it was not until 2005 that newer developments began to emerge. Some notable events included:
- 2006: Google BigTable paper published
- 2006: Amazon Web Services cloud platform launched
- 2007: Amazon Dynamo paper published
- 2008: Facebook makes Cassandra open source
- 2009: MongoDB released
- 2009: Redis released |
| 2010-2012 | 2010: NoSQL conferences and related events start gaining popularity and NoSQL becomes a commonly accepted technical term. At the same time, Hadoop becomes widely popular, and nearly all major companies begin the process of implementing Hadoop-related technologies.
2011: Market leaders start adopting Big Data and forming Big Data strategies. Numerous articles and research papers claiming the huge potential of Big Data makes it very popular. McKinsey publishes a paper on Big Data and calls it the next frontier of innovation, competition, and productivity. The October 2012 edition of, Harvard Business Review includes a very positive outlook on data scientists, which becomes immediately popular. |
| 2013-2015 | The growth of Big Data technologies leads to the development of a concept called data science, which moves the focus from just the data to the value of the data. Coupled with developments in machine learning and the rise of the popularity of R, Python, and other data science-oriented platforms, the industry shifts attention to getting insights from data as opposed to merely managing data. Machine learning is the new buzzphrase. |
| 2016- | The evolution of smart devices, wearables, AI-enabled cell phones, autonomous driving cars, and other such innovative solutions adds a new component of artificial intelligence to the existing trend of Big Data and machine learning. Manufacturers start broadly advertising the intelligent capabilities, as opposed to merely the machine learning capabilities, of technical solutions. |

The responsibility for implementing a Big Data platform or, more generally, a Big Data Initiative, is generally delegated to the IT or Analytics Department of a company, if such a department exists.
