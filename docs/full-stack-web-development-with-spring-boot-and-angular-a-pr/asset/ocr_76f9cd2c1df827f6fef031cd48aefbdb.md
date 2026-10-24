With that, we have learned about the importance of state management in developing applications, especially enterprise-level apps.

Now, let’s discuss more state management concepts and global states.

# State management and the global state

State management, as discussed in the previous section, is an extension or library that allows us to manage the flow of our data in the application in just one direction.

This is possible due to global states, which will contain all of the data. To understand how state management works, let’s discuss each of the building blocks of state management.

# Global state/store

The **global state**, also known as the **store**, is the most crucial element in state management. The primary responsibility of a global state is to store all the data retrieved by the API or simply data being used in the application.

This means that all components in an Angular application will retrieve the data in the global state. Think of it as a database of the Angular application but in the form of a JSON object where we can get each property as slices.

# Actions

**Actions** express unique events in your application. They are directly called in our components using the `dispatch()` function, which helps identify what events should be performed, such as modifying the state or calling APIs to retrieve data.

Actions are just simple interfaces; the `type` property identifies what action is dispatched. This simple string is just a definition of the action, and we can add properties to the actions for the data we require in the API or state.

Let’s have a look at an example of an action interface:

```
{
  type: '[Blog] Add Blog',
  title: string;
  author: string;
  content: string;
}
```