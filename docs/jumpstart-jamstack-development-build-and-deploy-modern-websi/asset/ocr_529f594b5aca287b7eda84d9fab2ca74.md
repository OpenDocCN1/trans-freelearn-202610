By adding sorting to the events query, the events are listed in chronological order. The following code demonstrates how this is done. Sorting is added to the dateAndTime field using the ASC (ascending) order:

```
events: allSanityEvent(
  sort: {
    fields: [dateAndTime]

    order: ASC
  },
) {
  ...
```

As shown in Figure 13.2, events that happened in the past, prior to September 1, 2020, in this case, are displayed with a darker background than events that will happen in the future. Let's learn how this effect is achieved.

First, node.dateAndTime is converted into a date by passing it as a parameter to the new Date() function. The current time is obtained by instantiating a date object without a parameter. The Boolean (true/false) result of the comparison, if the event date is less than the current date, is assigned to the pastEvent variable:

```
let pastEvent =
new Date(node.dateAndTime) < new Date() ? true : false;
```

Next, the background color is set based on whether pastEvent's value is set to True:

```
(${pastEvent ? 'bg-gray-600' : 'bg-gray-100'}):
return (
  <div className={`max-w-full shadow-sm mt-3 p-3 ${pastEvent ? 'bg-gray-600' : 'bg-gray-100'}`}>
  ...
```

Now that we've dealt with the styles, next, we will look at how to improve the code quality. We'll begin to prepare for deployment by first cleaning up the source code.

# Preparing for deployment

In this section, we will make several improvements to the code, making it cleaner. Additionally, we will improve the functionality of the form.