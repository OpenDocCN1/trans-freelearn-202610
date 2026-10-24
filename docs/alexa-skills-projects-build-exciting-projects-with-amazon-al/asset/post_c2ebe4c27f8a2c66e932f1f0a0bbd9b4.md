
## Configuring the Handsfree Messenger Skill

With our Twilio backend in place, we now need to configure the Alexa Skill. For this, we will configure via the Amazon Developer portal.

We will assume that the reader knows and has already signed up to the Amazon Developer portal, and is already aware of basic starter information about the Amazon Alexa Skills Developer portal. We have already covered account creation and other basic details for the Amazon Developer portal in the last chapter, hence we would not be repeating those here.

We will start with skill creation:

+   1. On the **Create a new skill** screen, please provide the skill with a name:

![](img/80ddccf2572659c5b147f23cc6356c5a_82_0.png)

Please save these changes and move on to the next screen by using the **Next** button at the top right of the screen.

+   2. In this step, we shall choose a model for our skill.

We have already covered the specifics of choosing a model in Chapter 2, *Hello World, Alexa!*, so we won't be going into much detail here. Please select a **Custom** model for the skill and move on to the next screen, the Skill builder checklist, after creating the Skill.

We already know from Chapter 2, *Hello World, Alexa!*, that an interaction model consists of intents, slots, and sample utterances. The interaction model of the Handsfree Messenger skill consists of all of those, which we will define in the following steps by completing the **Skill builder checklist**.

+   3. As the first step in the **Skill builder checklist**, we will provide an **Invocation Name** for our skill. The **Invocation Name** for our skill will be `handsfree messenger`. This is the name that users use to invoke our skill. Please save the model after providing the invocation name.

+   4. Next, we will need to configure an intent for our skill. Our skill consists of a single intent, `SendMessage`. We define the intent as follows:

![](img/80ddccf2572659c5b147f23cc6356c5a_83_0.png)

Please click on the **Create custom intent** button to create the intent.

But that is not all; we will also need to include a slot in the intent schema so that we can pass the message text as an argument to the Lambda.

## 第五章 树和二叉树

### 5.1 树的逻辑结构

树是n（n≥0）个结点的有限集合，在任意一棵非空树中：

+   1. 有且仅有一个特定的称为根的结点；

+   2. 当n>1时，其余结点可分为m（m>0）个互不相交的有限集合，其中每一个集合本身又是一棵树，并且称为根结点的子树。

树的逻辑结构具有以下特点：

+   （1）树的根结点没有前驱结点，除根结点之外的所有结点有且只有一个前驱结点。

+   （2）树中所有结点可以有零个或多个后继结点。

![](img/80ddccf2572659c5b147f23cc6356c5a_85_0.png)

图5-1 树结构示意图

## Coding the Lambda function

Once the interaction model has been created, we will leave this screen as is and will focus on creating the Lambda.

+   1. Please open a new browser tab and navigate to AWS console, the web URL for which is mentioned here:

The AWS console can be found at https://console.aws.amazon.com. Please note that you may need to create a new account if not already created.

We have already covered the creation of a basic Lambda function in Steps 8-12 in Chapter 2, Hello World, Alexa!, under the heading Hello Alexa – Building a Hello World Alexa Skill, so we shall not be repeating the steps here. Please follow those steps, name the Lambda function handsFreeMessengerSkill, and link the skill ID with the Lambda, as previously described in Chapter 2, Hello World, Alexa! under the heading Hello Alexa – Building a Hello World Alexa Skill.

Since our skill is very simple, our Lambda will be handling two basic requests:

- **Launch request**: To handle launching the skill
- **Intent request**: To handle various Intents (in our case, just one, SendMessage, to send the actual SMS)

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

2. Next up, we will define some helper functions that will help us in creating speech responses:

```
//------Helper Methods------//

function buildSpeechletResponse(output, shouldEndSession) {
    return {
        outputSpeech: {
            type: "PlainText",
            text: output
        },
        shouldEndSession: shouldEndSession
    };
}

function buildResponse(speechletResponse) {
    return {
        version: "1.0",
        response: speechletResponse
    };
}
```

These functions help the Lambda to return the response to be spoken by Alexa whenever it receives a specific intent/input.

The buildResponse function will generate the plainspoken response which shall eventually be voiced by Alexa.

The buildSpeechletResponse function will specify the output that Alexa will eventually speak, in the form of plain text. It also takes another variable, shouldEndSession, which specifies whether Alexa should end the session after the response or not.

3. Once we have the helper functions configured, we can update the handling of LaunchRequest and IntentRequest cases to have Alexa speak with relevant responses, as shown here:

```
exports.handler = function (event, context) {
    try {
        if (event.request.type === "LaunchRequest") {
            onLaunch(function callback(speechletResponse) {
                context.succeed(buildResponse(speechletResponse));
            });
        } else if (event.request.type === "IntentRequest") {
            onIntent(event.request,
                function callback(speechletResponse) {
```

```
```javascript
context.succeed(buildResponse(speechletResponse));
});
}
} catch (e) {
    context.fail("Exception: " + e);
}
};
```
```

Now, when our `Lambda` receives a `LaunchRequest`, it will call the `onLaunch` method to send a welcome message back, which we will implement in the next step.

Also, if it receives an `IntentRequest`, it can call the `onIntent` method, which in turn will filter down on the intent's name (although filtering is not needed currently since we have only one custom intent).

Save your progress by clicking **Save** in the top-right corner of the screen.

+   4. We shall now implement the `onLaunch` and the `onIntent` methods that we discussed in the previous step:

```
```javascript
function onLaunch(callback) {
    var output = "Welcome to HandsFree Messenger.";
    var endSession = false;
    callback(buildSpeechletResponse(output, endSession));
}

function onIntent(intentRequest, callback) {
    var intent = intentRequest.intent,
        intentName = intentRequest.intent.name;
    if("SendMessage" === intentName){
        var text = intent.slots.Text.value;
        var recipient = '+31XXXXXXXXXX';
        SendMessage(recipient, text, callback);
    } else {
        throw "Intent not recognized";
    }
}
```
```

The `onLaunch` method is simple—it creates a welcome message and sends it back after creating a speech response out of that message, using the helper methods we defined earlier.

The onIntent method is comparatively tricky—it filters out the intent based on its name and if the intent is the SendMessage intent, then it extracts the SMS text from the slot value before forwarding it to the recipient's hard-coded mobile number via the SendMessage function.

We shall define the SendMessage function in the coming steps.

+   5. The SendMessage function is the juice of the whole skill. It is the one responsible for passing the message to the Twilio backend. However, before defining the SendMessage function, we need to declare a few constants that will be required while interacting with the Twilio API.

If you remember, we noted down the SID and the Auth Token in the last step of the previous section; it is time now to declare them. We will be using these constants in the SendMessage function. Declare those variables as shown here:

```
var sid = 'ACXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX';
var token = '4bXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX';
var sender = '+31XXXXXXXXXX';

var https = require('https');
var queryString = require('querystring');
```

Define these constants at the very top of the index.js file on the AWS developer portal. After defining them, the index.js file on the developer portal should resemble the one shown here:

![](img/80ddccf2572659c5b147f23cc6356c5a_89_0.png)

Figure 3.20: Coding the Lambda
