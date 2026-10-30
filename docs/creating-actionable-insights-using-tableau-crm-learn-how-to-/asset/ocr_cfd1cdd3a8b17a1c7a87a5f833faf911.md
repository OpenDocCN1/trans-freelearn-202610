![](bbox=[0.115, 0.11, 0.85, 0.52])

Figure 9.15 – JSON and the TCRM dashboard

When in the dashboard editor, press Cmd + E (or Ctrl + E) to show the JSON behind the dashboard. JSON is just the language in which the instructions are written. The JSON contains all the widgets/charts descriptions and the queries to run against the datasets. Each chart is actually several lines of JSON describing the types and properties and the query to run. A special case is when one of these queries is custom-written directly, using SAQL.