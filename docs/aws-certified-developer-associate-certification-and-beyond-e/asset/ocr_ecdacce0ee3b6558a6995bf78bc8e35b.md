If the maximum number of retries have been attempted and the error still persists, then Step Functions initiates a Catch error-handling option. This is also known as a fallback state and is applicable to the Task, Map, and Parallel states. For the Catch field, you must define an array of objects, known as catchers.

Catch has two key fields, ErrorEquals and Next, as depicted in Figure 15.19:

```
"HelloWorld": {
  "Type": "Task",
  "Resource": "arn:aws:lambda:REGION:ACCOUNT_ID:function:FUNCTION_NAME",
  "Catch": [
    {
      "ErrorEquals": ["States.Timeout"],
      "Next": "fallback"
    }
  ],
  "End": true
},
"fallback": {
  "Type": "Pass",
  "Result": "This is the fallback from the catch",
  "End": true
},
```

Figure 15.19 – Step Functions error handling – Catch

In this example, if the timeout error is encountered and either the retry option is not available or the maximum number of retries have been attempted, then the next step is to perform the activity indicated in the Next field, which is to go to the fallback state.

Additionally, when you use a state machine to catch an error, you can preserve the original input with the error, which can be passed into the state specified in the Next field. So, in Figure 15.20, ResultsPath in Catch will include the error with the original input.

```
"HelloWorld": {
  "Type": "Task",
  "Resource": "arn:aws:lambda:REGION:ACCOUNT_ID:function:FUNCTION_NAME",
  "Catch": [
    {
      "ErrorEquals": ["States.Timeout"],
      "Next": "fallback",
      "ResultsPath": "$.error"
    }
  ],
  "End": true
},
"fallback": {
  "Type": "Pass",
  "Result": "This is the fallback from the catch",
  "End": true
},
```

Figure 15.20 – ResultsPath in the Catch statement