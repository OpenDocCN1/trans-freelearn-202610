I want to share one final example of where I used code – in this case, customizing the JSON data flow to reset and refresh dates every time the data flow was run:

```
"relativeDates2": {
    "action": "computeExpression",
    "parameters": {
        "source": "substituteNewDate",
        "mergeWithSource": true,
        "computedFields": [
            {
                "defaultValue": "0",
                "precision": 18,
                "name": "dateAsEpochSec2",
                "saqlExpression": "case when 'Date_of_cancellation' is null then null else Date_of_cancellation_sec_epoch end",
                "scale": 0,
                "label": "dateAsEpochSec2",
                "type": "Numeric"
            },
            {
                "defaultValue": "0",
                "precision": 18,
                "name": "dateDifference2",
                "saqlExpression": "case when 'Date_of_cancellation' is null then null else dateAsEpochSec2 - 1522127025 end ",
                "scale": 0,
                "label": "dateDifference2",
                "type": "Numeric"
            },
            {
                "defaultValue": "0",
                "precision": 18,
                "name": "dateAsEpoch2",
                "saqlExpression": "case when 'Date_of_cancellation' is null then null else dateDifference2 + date_to_epoch(now()) end ",
                "scale": 0
```