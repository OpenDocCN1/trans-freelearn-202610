# Coding the Lambda function

Once the interaction model has been created, we will leave this screen as is and will focus on creating the Lambda.

+   1. Please open a new browser tab and navigate to AWS console, the web URL for which is mentioned here:

The AWS console can be found at https://console.aws.amazon.com. Please note that you may need to create a new account if not already created.

We have already covered the creation of a basic Lambda function in Steps 8-12 in Chapter 2, Hello World, Alexa!, under the heading Hello Alexa – Building a Hello World Alexa Skill, so we shall not be repeating the steps here. Please follow those steps, name the Lambda function handsFreeMessengerSkill, and link the skill ID with the Lambda, as previously described in Chapter 2, Hello World, Alexa! under the heading Hello Alexa – Building a Hello World Alexa Skill.

Since our skill is very simple, our Lambda will be handling two basic requests:

+   - **Launch request**: To handle launching the skill
+   - **Intent request**: To handle various Intents (in our case, just one, SendMessage, to send the actual SMS)

We will be editing the code inline, in the index.js file in the developer portal itself. We shall start by segregating each of the requests that our Skill receives. For this, please declare the following handler:

```
exports.handler = function (event, context) {
    try {
        if (event.request.type === "LaunchRequest") {
        } else if (event.request.type === "IntentRequest") {
        }
    } catch (e) {
        context.fail("Exception: " + e);
    }
};
```

This is how we segregate the launch and intent event requests.