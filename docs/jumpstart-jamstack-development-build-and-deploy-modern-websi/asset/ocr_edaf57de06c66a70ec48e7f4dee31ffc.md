The fetch() function requires a **Graph Oriented Query (GROQ)**. In our case, we want five documents of the event type where the event date is greater than the current date. We also prepare the list array where we can add our results and the speakOutput variable, where we can add the message. Then we use the fetch() function to get the results:

```
```javascript
const GetUpcomingEventsIntentHandler = {
    canHandle (handlerInput) {
        ...
    }
    async handle (handlerInput) {
        const sanityClient = require('@sanity/client')
        const client = sanityClient({
            projectId: 'your-project-id',
            dataset: 'your-dataset',
            useCdn: true
        });
        const query = '*[_type == "event"]'
        let speakOutput
        let list = [];
        await client.fetch(query).then(events => {
            events.forEach(event => {
                list.push(event.name)
            })
        })
    }
};
```
```

Now, if there are upcoming events, they will be added to the list array.

+   8. We can now build the response:

```
```javascript
const GetUpcomingEventsIntentHandler = {
    canHandle (handlerInput) {
        ...
    }
    async handle (handlerInput) {
        const sanityClient = require('@sanity/client')
        const client = sanityClient({
```
```