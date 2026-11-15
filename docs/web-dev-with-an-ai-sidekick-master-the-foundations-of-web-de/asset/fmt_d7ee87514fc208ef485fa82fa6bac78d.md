
```javascript
try {
  const message = greet("");
  console.log(message);
} catch (error) {
  console.error(error.message);
}
```

This pattern makes it clear when something is wrong and allows you to respond appropriately.

You can also add a `finally` block. This always runs, whether an error occurred or not:

```javascript
try {
  console.log("Trying something...");
} catch (error) {
  console.error("It failed:", error);
} finally {
  console.log("This always runs");
}
```

Use `finally` for cleanup tasks, such as resetting UI state or stopping a loading indicator.

## When to Use Error Handling

A common beginner mistake is overusing `try` and `catch`.

Use it when:

- You are calling code that might fail unpredictably
- You want to prevent the entire program from stopping
- You need to recover or show a user-friendly message

Avoid it when:

- A simple conditional check would prevent the error
- You are using it to hide bugs instead of fixing them

For example, this is better:

```javascript
if (element) {
  element.textContent = "Updated";
}
```

Than this:

```javascript
try {
  element.textContent = "Updated";
} catch {}
```

The second version hides the problem instead of addressing it.

**Good practice**: Think of error handling as a safety net, not a primary control flow tool.

Most of the time, you should:

- Write code that avoids errors (by checking values)
- Use the console to debug when something goes wrong
- Add `try`/`catch` when you need to handle unexpected failures gracefully

As your programs grow, errors will become more subtle. Some will come from user input. Others will come from missing elements, timing issues, or unexpected data.

Being able to catch and handle errors means your code can respond intelligently instead of breaking in confusing ways. It also makes AI-generated code safer to test because you can isolate the part that may fail and inspect the actual error.

## Putting It Together with Events

This section ties the basics together without going deep into the DOM yet. It uses variables, functions, conditionals, and event handlers.

In `index.html`, after the existing `<script>` elements, add a new `<script>` element to reference a new code file named `events.js`, as shown in the following code:

```html
<script src="events.js"></script>
```

In the same folder as the HTML file, create the code file named `events.js`, and enter the following code:

```javascript
const counterElement = document.querySelector("#counterValue");
const incrementBtn = document.querySelector("#incrementBtn");
const resetBtn = document.querySelector("#resetBtn");
statusElement.textContent = "Status: Putting it together.";
let counter = 0;
function renderCounter() {
  counterElement.textContent = String(counter);
  messageElement.textContent = `Message: Counter is now ${counter}.`;
}
incrementBtn.addEventListener("click", () => {
  counter = counter + 1;
  renderCounter();
});
resetBtn.addEventListener("click", () => {
  counter = 0;
  renderCounter();
});
// Initial render so the UI matches the variable.
renderCounter();
```

Refresh the web page and note that every time you click the **Increment counter** button, the counter increments by one. After clicking the button three times, the output is **Counter: 3**, as shown in *Figure 5.7*:

![](Images/B34553_05_07.png)

Figure 5.7: Incrementing a counter with button click events

This is the shape of most interactive web code. State lives in variables like `counter`, behavior lives in functions like `renderCounter`, and user actions on a web page trigger events.

AI can produce JavaScript quickly. That is not the difficult part. The difficult part is deciding what should happen when the user clicks, types, submits, or changes something. In the counter example, you need to know where the number is stored, which button changes it, and which part of the page must be updated afterward. That is the skill to practice: follow the moving parts, then decide whether the code matches the behavior you wanted.

Now that you've been introduced to JavaScript and the main components of the language, let's test what you've learned with a mini exercise.

### Mini Exercise and Quiz

Use the same page and make some small modifications and additions:

- Change the counter step to add 5 each time
- Add a "Decrement" button
- Change the greeting to always use uppercase

**Prompt**: Please quiz me about JavaScript basics like declaring variables and writing simple functions. For example, "What does `const` vs `let` do?"

In the next section, we will connect these basics to the browser itself by looking at the DOM. That is where variables, functions, and events come together to change what the user sees and interacts with on the page.

# DOM Manipulation

In the `Chapter05` folder, create a folder named `DomManipulation`, and in it create a file named `index.html` with the following code:

You can download the source code for this web page from the following link: [https://github.com/markjprice/web-dev-ai/blob/main/Code/Chapter05/DomManipulation/index.html](https://github.com/markjprice/web-dev-ai/blob/main/Code/Chapter05/DomManipulation/index.html)
