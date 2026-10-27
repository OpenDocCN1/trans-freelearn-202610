```
```r
# Define server logic required to draw a histogram
server <- function(input, output, session) {
  h <- open_connection("localhost", "5001")
  minDate <- execute(h,"minDate")
  maxDate <- execute(h,"maxDate")
  startDate <- minDate
  endDate <- startDate + 31
  cmsdata <-
  data.table(dbColumns=c("month","date","firstName","lastName","City","State","Company","Product","Category","Payment","PaymentNature"),
  webColumns=c("Month","Date","First Name","Last Name","City","State","Company","Product","Category","Payment","Payment Nature"))
  companyData <- execute(h,"exec distinct showCompany from alldata")
  gbyVars <- c("Company","Product","State","City","Category","Payment Nature")

  PLACEHOLDERLIST <- list(
    placeholder = 'Please select an option below',
    onInitialize = I('function() { this.setValue(""); }')
  )

  PLACEHOLDERLIST2 <- list(
    placeholder = 'Select All',
    onInitialize = I('function() { this.setValue(""); }')
  )

  output$month <- renderUI({
    dateRangeInput("date", label = 'PAYMENT DATE', start = startDate, end = endDate, min = minDate, max = maxDate)
  })

  output$company <- renderUI({
    selectizeInput("company","COMPANY" , companyData, multiple = TRUE, options = PLACEHOLDERLIST)
  })

  output$product <- renderUI({
    productQuery <-
    paste0("getShowInfo(`product;\\\"",paste(input$company,collapse="|"),"\\\")")
    productVals <- execute(h,productQuery)
    selectizeInput("product", "DRUG/PRODUCT" , productVals, multiple = TRUE,options = PLACEHOLDERLIST2)
  })

  output$state <- renderUI({
    stateQuery <-
    paste0("getShowInfo(`state;\\\"",paste(input$company,collapse="|"),"\\\")")
    stateVals <- execute(h,stateQuery)
    selectizeInput("state", "STATE" , stateVals, multiple = TRUE,options = PLACEHOLDERLIST2)
  })
}
```
```