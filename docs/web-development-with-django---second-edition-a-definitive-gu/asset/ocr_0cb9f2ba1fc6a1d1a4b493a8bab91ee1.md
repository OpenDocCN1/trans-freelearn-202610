7. Start the Django dev server if it is not already running, then go to http://127.0.0.1:8000/react-example/. Try clicking the button a few times – it should increment until you click it the target number of times. Then, it will be replaced with the Well done, <name>! text. See Figure 16.13 for how it should look after you’ve clicked it enough times:

![](bbox=[0.11, 0.18, 0.85, 0.31])

Figure 16.13 – The Well done message

In this exercise, we passed data to a React component using props. We escaped the data when assigning it to a JavaScript variable using the escapejs template filter. In the next section, we will cover how to fetch data over HTTP using JavaScript.

**Further reading**
For a more detailed, hands-on course on React, you can always refer to *The React Workshop*: https://courses.packtpub.com/courses/react.

# JavaScript promises

Many JavaScript functions are implemented asynchronously to prevent blocking on long-running operations. They work by returning immediately but then invoking a callback function when a result is available. The object these types of functions return is a Promise object. Callback functions are provided to the Promise object by calling its then method. When the function finishes running, it will either resolve the Promise (call the success function) or reject it (call the failure function).

We will illustrate the wrong and right ways of using promises. Consider a hypothetical long-running function that performs a big calculation called getResult. Instead of returning the result, it returns Promise. You would not use it like this:

```
const result = getResult();
console.log(result); // incorrect, this is a Promise
```