4. In the index.js page, after HelloWorldIntentHandler, we can add GetUpcomingEventsIntentHandler:

5. Let's start from the canHandle() function. We know that the request type must be an IntentRequest and that our intent name is GetUpcomingEventsIntent, so we write the following:

```
const GetUpcomingEventsIntentHandler = {
    canHandle(handlerInput) {
        return Alexa.getRequestType(handlerInput.requestEnvelope) === 'IntentRequest'
            && Alexa.getIntentName(handlerInput.requestEnvelope) === 'GetUpcomingEventsIntent';
    },
};
```

6. Now we can implement the handle() function. Because the Sanity Client returns a Promise, the handle() function must be asynchronous.

The first step is to initialize the Sanity Client:

```
const GetUpcomingEventsIntentHandler = {
    canHandle(handlerInput) {
        ...
    }
    async handle(handlerInput) {
        const sanityClient = require('@sanity/client')
        const client = sanityClient({
            projectId: 'your-project-id',
            dataset: 'your-dataset',
            useCdn: true
        });
    }
};
```

7. Now we can fetch the data using the fetch() function from the Sanity Client API.