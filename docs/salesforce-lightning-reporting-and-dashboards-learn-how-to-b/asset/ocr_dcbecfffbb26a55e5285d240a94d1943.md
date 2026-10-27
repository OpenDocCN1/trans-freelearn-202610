For a custom object, once you enable **Track Field History** in the object details, create a new custom report, and notice the report type ending with `History` is available for you to use, such as `Claim History`. Similar to the aforementioned **Account History**, you can use this report type as a normal report. Consider the following screenshot for the report:

| HISTORY ID | PARENT ID | ACCOUNT NAME | EDITED BY | EDIT DATE | FIELD / EVENT | OLD VALUE | NEW VALUE |
|---|---|---|---|---|---|---|---|
| 017B000002tGNh | 001B000000SWOQO | Electric Inc Limited | Alvin Lee | 5/24/2017 9:10 PM | Account Number | - | 8001 |
| 017B000002tGfz | 001B000000SWOQO | Electric Inc Limited | Johan Yu | 5/24/2017 9:10 PM | Account Owner | Johan Yu | Alvin Lee |
| 017B000002tGcL | 001B000000SWOQO | Electric Inc Limited | Johan Yu | 5/24/2017 9:09 PM | Description | - | - |
| 017B000002tGcK | 001B000000SWOQO | Electric Inc Limited | Johan Yu | 5/24/2017 9:08 PM | Account Name | Electric Inc | Electric Inc Limited |
| 017B000002tGyA | 001B000000SWOQO | Electric Inc Limited | Johan Yu | 5/24/2017 9:08 PM | Created | - | - |
| 017B000002tGVC | 001B000000SWOQJ | Big Inc. Limited | Johan Yu | 5/24/2017 9:07 PM | Description | - | - |
| 017B000002tGVB | 001B000000SWOQJ | Big Inc. Limited | Johan Yu | 5/24/2017 9:07 PM | Account Name | Big Inc. | Big Inc. Limited |
| 017B000002tGWM | 001B000000SWOQJ | Big Inc. Limited | Johan Yu | 5/24/2017 9:07 PM | Created | - | - |

# Limitations of using field history tracking

Some of the limitations have been explained previously, but let us summarize them here:

+   - By default, only a maximum of 20 fields can be tracked
+   - For **Text Area (Long)**, **Text Area (Rich)**, and **Picklist (Multi-Select)** type of field, you cannot track the original and new value
+   - Not all standard objects can be tracked; for example, **User**, **Task**, **Event**, **Campaign**, and **Quote**
+   - Not all fields can be tracked; for example, the **Formula** field, **Roll-Up Summary**, auto-number fields, **Created By**, **Last Modified By**, and **Expected Revenue** fields on **Opportunity**

# Reporting on historical data with Reporting Snapshots

Can we have a report on data trending in Salesforce? Does Salesforce only report current data? Field history tracking will store old and new values whenever value changes in fields are detected. But to use it for trending would be too difficult, unless you export the data to an external database and analyze it with live data--it will not include deleted data either (if any).