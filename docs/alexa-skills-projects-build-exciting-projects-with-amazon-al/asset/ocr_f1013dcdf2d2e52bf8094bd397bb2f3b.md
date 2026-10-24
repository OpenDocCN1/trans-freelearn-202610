```
if ("OneShotFetchTemperatureIntent" ===
    event.request.intent.name) {
        fbGet("/Temperature").then(res => {
            context.succeed(buildResponse(buildSpeechletResponse("The temperature is . . " + res + " . degrees", true)));
        })
    } else {
        throw "Invalid intent";
    }
}
catch(e) {
    context.fail("Exception: " + e);
}
```

Upon a successful fetch, we create a speech response and pass it back to be spoken by Alexa. This completes our Lambda implementation.

+   4. We need to link the Lambda to our skill and for this purpose, we will need the Amazon Resource Number (ARN) of our Lambda. The ARN is located in the top-right corner of the Lambda function/code editor page:

![](bbox=[0.1, 0.42, 0.9, 0.7])

Figure 6.34: Copying the ARN