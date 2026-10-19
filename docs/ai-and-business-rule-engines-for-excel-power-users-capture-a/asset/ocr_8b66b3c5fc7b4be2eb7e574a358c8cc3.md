There are a few key Excel items in this spreadsheet that are worth highlighting:

+   - The area highlighted in gray is a named range/table called `ParamsTable`. This contains the information that we want to pass to our decision service to get a recommendation on the type of chocolate or candy bar that might be suitable for us.
+   - In the Params table, we can easily update the `Rest_api` value. It's important that you edit it now to a valid endpoint for your own OpenShift instance; otherwise, this sample won't run. It's the same value as we used for `RestUrl` in the previous example.
+   - The second worksheet tab, `1 DS no Param`, is the output from the hardcoded REST call in our previous example.
+   - The third worksheet tab, `2 DS with Params`, gives the output from this example when we run it.

If you open the Power Query editor, you'll see that we've followed our own advice and kept several earlier queries to help with debugging. Please note that the entire formula is still contained in a single query called **2 Param-decision-service**; otherwise, the formula firewall will stop us from mixing internal and external data sources.

Other queries in our sample are not essential but useful to have, for when you evolve this sample to use in your day-to-day work:

+   - **1 DS No Params** is our earlier hardcoded query. That's useful to confirm our decision service is accepting POST requests but not used in the final query.
+   - The main example is **2 DS With Params**. Let's open that in Advanced Editor in Power Query and walk through it step by step. The first few lines of the following script get the URL of our decision service from the spreadsheet and put it into a variable:

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