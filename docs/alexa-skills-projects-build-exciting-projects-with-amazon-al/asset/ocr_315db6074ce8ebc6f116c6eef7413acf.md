We will be editing the code inline on the Developer portal itself. We will start by segregating each of the requests that our skill receives:

```
```javascript
exports.handler = (event, context, callback) => {
    switch (event.request.type) {
        case "LaunchRequest":
            break;
        case "IntentRequest":
            break;
    }
}
```
```

As shown in the preceding code, this is how we segregate the launch and intent event requests.

+   15. Next, we will define some helper functions that will help us to create speech responses:

```
```javascript
buildSpeechletResponse = (outputText, shouldEndSession) => {
    return {
        outputSpeech: {
            type: "PlainText",
            text: outputText
        },
        shouldEndSession: shouldEndSession
    }
}

generateResponse = (speechletResponse) => {
    return {
        version: "1.0",
        response: speechletResponse
    }
}
```
```