
+   4. You will need to update the endpoint listed in the line starting `RestUrl` with the value of the one in your deployment of OpenShift. As a reminder, it’s the value we told you to make a note of in the *Some vital information* section – the URL starting ... `https://dmn-dev-sandbox-...`. Pay attention to quotation marks when editing this value in Power Query.

+   5. When you have completed editing, click on **Done**, then **Refresh Preview** in Power Query. If you have forgotten to refresh your Power Apps key today, or you have entered an incorrect value for `RestUrl`, you will get a `[DataSource.Error] Web.Contents failed to get contents from ...` error. If so, go back to *step 4* and correct this.

+   6. The first time you use this query, Power Query will ask whether you need to log in to use this service, as in *Figure 6.18*. For all our examples, **Anonymous** access is okay – but if you deployed this service in real life, you would want to review the security and login options. Click on **Connect** to exit this dialog.

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_173_0.png)

Figure 6.18 – Power Query login check

+   7. Congratulations! We get the expected response from the decision service. A **Potential Health Issue – please check** message should be displayed in the main area of the Power Query window.

Much of what is happening in this query we can guess – since it is merging the REST calls we saw in Swagger with the Power Query formula format. But it’s worth highlighting the key steps we’ve just taken in this query:

+   1. Note that each line in the `in` section is separated by a comma. That is how Power Query knows that each line is a separate step. Be careful when editing, as Power Query will complain if you remove it!

+   2. As we saw (and edited), the line starting `RestUrl = "http:// ..."` sets the REST endpoint we want to call. We save it as a variable as it makes it easier to edit later.

+   3. We create another variable, `Payload`, which is almost exactly the same JSON text as we used before in Swagger. Since this text needs to have quotes in it, we’ve had to use double quotes so that M doesn’t get confused and think that we’ve finished the line prematurely.

+   4. The line `Response = ...` uses the `Web.Contents` connector from Power Query to do the POST call. You’ll see that this connector allows us to set the headers needed by our decision service.
+   5. After the call is complete, `JsonResult = Json.Document(Response)` formats the text returned so that Power Query knows we’re using a JSON format.
+   6. The final `in` part of the query specifies which of the variables we want to return as the result of our query.

It’s great that we’ve finally connected Excel to our decision service using Power Query. But while we can edit the query, that’s not practical for day-to-day use. It’s very easy to break this query when editing by putting a comma or double quote in the wrong place. Let’s fix that, using the knowledge of Power Query parameters that we learned earlier.

## Calling the decision service using parameters

In this section, we’re bringing all the parts together – REST calls and parameters from Excel into Power Query, making a POST call, then bringing the information back to Excel. Since we’ve already walked through the individual steps, it makes sense to give you a complete downloadable sample at `06_power_query_decision_service.xls` on the book’s GitHub sample site. This allows us to highlight the key areas to allow you to extend the approach for your own work.

If you open the example, you’ll see the main sheet, and two other worksheet tabs, as in *Figure 6.19*.

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_174_0.png)

Figure 6.19 – Main sheet and other tabs of our full sample

There are a few key Excel items in this spreadsheet that are worth highlighting:

- The area highlighted in gray is a named range/table called `ParamsTable`. This contains the information that we want to pass to our decision service to get a recommendation on the type of chocolate or candy bar that might be suitable for us.
- In the Params table, we can easily update the `Rest_api` value. It's important that you edit it now to a valid endpoint for your own OpenShift instance; otherwise, this sample won't run. It's the same value as we used for `RestUrl` in the previous example.
- The second worksheet tab, `1 DS no Param`, is the output from the hardcoded REST call in our previous example.
- The third worksheet tab, `2 DS with Params`, gives the output from this example when we run it.

If you open the Power Query editor, you'll see that we've followed our own advice and kept several earlier queries to help with debugging. Please note that the entire formula is still contained in a single query called **2 Param-decision-service**; otherwise, the formula firewall will stop us from mixing internal and external data sources.

Other queries in our sample are not essential but useful to have, for when you evolve this sample to use in your day-to-day work:

- **1 DS No Params** is our earlier hardcoded query. That's useful to confirm our decision service is accepting POST requests but not used in the final query.
- The main example is **2 DS With Params**. Let's open that in Advanced Editor in Power Query and walk through it step by step. The first few lines of the following script get the URL of our decision service from the spreadsheet and put it into a variable:

```
let
    // the Rest URL we are calling - you will need to
    //update this with the value from the Swagger API
    // This example takes it from Excel
    RestUrl = Excel.CurrentWorkbook(){[Name="ParamsTable"]}
    [Content]{0}[Value],
```

The next lines are new compared to our previous hardcoded example – we read our parameters from Excel into M variables. This keeps building the JSON string (in the next step) clean and easy to read:

```
//We set our values into variables and then build the
//JSON - it means we are less likely to break the
//Payload formula.
```

```
pCustomerNumber=Excel.CurrentWorkbook()
{[Name="ParamsTable"]}[Content]{1}[Value],
    pDateOfBirth=Excel.CurrentWorkbook(){[Name="ParamsTable"]}
[Content]{3}[Value],
    pCountryOfResidence=Excel.CurrentWorkbook()
{[Name="ParamsTable"]}[Content]{4}[Value],
    pSpecialRequests=Excel.CurrentWorkbook()
{[Name="ParamsTable"]}[Content]{5}[Value],
```

The following step builds the JSON payload string using the values we have just read. While the triple quote marks """ look strange, they are needed to get the JSON in the correct format.

In practice, editing this when you build your own decision service is easier than it looks – you’ll replace the field name (for example, Number), taking care not to disturb the quotes. And if you do encounter an issue, see the troubleshooting section on how to quickly find and fix errors:

```
// The JSON values we will pass in to call the decision
// service
Payload = "

{""Customer"": {
    ""Number"": "" & pCustomerNumber & "",
    ""Name"": ""Joe Bloggs"",
    ""Date of Birth"": "" & pDateOfBirth & "",
    ""Country of Residence"": """ & pCountryOfResidence & """,
    ""Special Requests"": """ & pSpecialRequests & """,
    ""Previous Orders"": [0]
}}
"
,
```

Our JSON call is almost exactly as before, as is the final in part of the query to display the results:

```
// Do the JSON Call

Response= Web.Contents(RestUrl,
    [
        Content=Text.ToBinary(Payload),
        Headers=[ #"Content-Type" = "application/json",
                  #"Accept" = "application/json"]
```

```
]
),

//Convert the response for display
JsonResult = Json.Document(Response)

// display the results

in
    JsonResult
```

To use this example to call our chocolate shop decision service, no knowledge of Power Query is needed. Close the Power Query editor and return to Excel, then take the following steps:

+   1. Enter the values in the table on the first screen – make sure you have a valid endpoint. The endpoint would normally be hidden from users, but it makes our sample easier for now.
+   2. Go to the output tab for our example, **2 DS with Params**.
+   3. Right-click on the table and click refresh to update the table – there are options available to do this automatically every couple of minutes.
+   4. The decision service will process the values provided and pass back the recommendation to be shown in the Excel table (as in Figure 6.20). As expected for the sample, we recommend **Silk Tray** products to customers who order early, and the more generic **Milk Chocolate**. If you have edited your decision model before and deployed it, your results may vary.

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_177_0.png)

Figure 6.20 – The results of our decision service call in Excel

This might appear to be a simple step, but it is a key milestone as we approach the halfway point of our book. We have been able to take knowledge previously embedded in Excel, deploy it into a decision service, take advantage of business rules and AI, and link that service back to Excel so that most users will still be able to access it.
