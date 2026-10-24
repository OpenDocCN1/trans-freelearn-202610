```
"France",
"Austria",
"Hungary",
"Germany"
]
},
{
"The battle of Hastings was fought in which country?":
[
"France",
"Russia",
"England",
"Norway"
]
}
];

var correctAnswerSlots = [3, 1, 2];
```

As shown in the preceding code, apart from the questions array, we have also defined another array called correctAnswerSlots, which contains the index of the correct answers for each question defined in the questions array.

Confused? We can elaborate this with an example. It can be deduced from the correctAnswerSlots array that the answer to the first question, World War I began in which year?, is 1914, which is located at index 3 (or is the fourth choice). Hence, we have 3 in the first position in the correctAnswerSlots array.

+   2. Now, we can go ahead and implement the handling of events in exports.handler. Our Lambda will be handling two basic requests:
+     - LaunchRequest: To handle the launch of the skill
+     - IntentRequest: To handle various Intents (in our case, we have just two intents, the first one being the Answer intent, which indicates the answer of the quiz question posed by Factly, and AMAZON.StopIntent, which indicates an exit from the skill)

We shall start by segregating each of the requests that our skill receives. For this, declare the following handler:

```
exports.handler = function (event, context) {
    try {

        if (event.request.type === "LaunchRequest") {
        } else if (event.request.type === "IntentRequest") {
```