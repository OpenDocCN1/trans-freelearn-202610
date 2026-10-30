```
q = foreach q generate 'Bracket' as 'Bracket', 'Account_Name' as 'Account Name', max('sum_Revenue') as 'sum_Revenue';
q = order q by 'sum_Revenue' desc;
```

Another example is where you want to calculate the average deal size and use this to create dynamic reference lines:

```
"query": "q = load \"Opportunity_MT\";\nq = filter q by 'Type' == \"New WM Client\";\nq = filter q by 'StageName' == \"Revenue Received\";\nq = group q by ('CloseDate_Year', 'CloseDate_Quarter', 'AccountId.Name');\nq = foreach q generate 'CloseDate_Year' + \"~~~\" + 'CloseDate_Quarter' as 'CloseDate_Year~~~CloseDate_Quarter', 'AccountId.Name' as 'Household', sum('Amount') as 'Amount';\nq = group q by ('CloseDate_Year~~~CloseDate_Quarter');\nq = foreach q generate 'CloseDate_Year~~~CloseDate_Quarter' as 'CloseDate_Year~~~CloseDate_Quarter', trunc(avg('Amount'),0) as 'Average';\nq = group q by all;\nq = foreach q generate trunc(avg('Average'),-2) as 'Avg Amount';",
"referenceLines": [
{
"color": "#9271E8",
"label": "Target = {{cell(gaugeRef6_1.result,0,\"target\").asString()}}",
"value": "{{cell(gaugeRef6_1.result,0,\"target\").asString()}}"
},
{
"color": "rgb(0, 161, 224)",
"label": "Avg = {{cell(Avg_of_Avg_Deals_Siz_1.result, 0,\"Avg Amount\").asObject()}}",
"value": "{{cell(Avg_of_Avg_Deals_Siz_1.result, 0,\"Avg Amount\").asObject()}}"
}
],
```

# Using images as icons

Images can be used as icons to add to the aesthetics and make it more intuitive to drill down into user data, as shown in the following screenshot: