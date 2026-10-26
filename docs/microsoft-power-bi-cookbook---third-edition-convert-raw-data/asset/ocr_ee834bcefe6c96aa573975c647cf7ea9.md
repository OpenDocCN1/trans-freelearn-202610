3. Configure the streaming semantic model to align with the columns and data types of the FactSurveyResponse table.

# New streaming dataset

Create a streaming dataset and integrate our API into your device or application to send data. Learn more about the API.

* Required

Dataset name *
Customer Survey Response

Values from stream *
SurveyResponseKey  Number
CustomerKey  Number
ProductCategory  Text
ProductSubcategory  Text
Date  DateTime
Enter a new value name  Text

```
[
  {
    "SurveyResponseKey" : 98.6,
    "CustomerKey" : 98.6,
    "ProductCategory" : "AAAAA555555",
    "ProductSubcategory" : "AAAAA555555",
    "Date" : "2024-02-09T22:00:15.381Z"
  }
]
```

Historic data analysis
On

Figure 12.17: Streaming semantic model configuration

4. Enter a semantic model name, enable the Historic data analysis setting, and click the Create button.

5. Copy the Push URL value and click the Done button.

6. Log in to Power Automate in Microsoft 365: https://make.powerautomate.com.