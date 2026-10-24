3. Once you have clicked the **Save** button, select the `factlySkill` box under the **Designer** section, as shown in the following screenshot:

![](bbox=[0.105, 0.177, 0.895, 0.412])

Figure 4.26: Concluding the Lambda configuration

This should bring up a code editor at the bottom where we will add the code for the Lambda.

# Coding the Lambda function

We will be editing the code inline, in the `index.js` file in the developer portal itself:

+   1. First of all, we shall define an array of JSON, which will contain the quiz questions to be posed by the trivia skill. This can be defined right at the top of the `index.js` file, even before the `exports.handler`:

```
```javascript
//questions is an array of json objects
var questions = [
    {
        "World War I began in which year?": [
            "1923",
            "1938",
            "1917",
            "1914"
        ]
    },
    {
        "Adolf Hitler was born in which country?": [
```
```