
+   - `super(props)` calls the super class's constructor method and passes the props variable. This is analogous to the `super()` method in Python.
+   - Each React class has a state variable, which is an object. `constructor` can initialize it. The state should be changed using the `setState` method rather than being manipulated directly. When it is changed, the `render` method will be automatically called to redraw the component.

The `render` method returns a new HTML element using the `React.createElement` function (remember, the e variable was aliased to this function). In this case, the arguments to React.createElement will return a <button> element with a click handler and with the `this.state.clickCount` text content. Essentially, it will return an element like this (when clickCount is 0):

```
<button onClick="this.setState(...)">
  0
</button>
```

The onClick function is set as an anonymous function with arrow syntax. This is similar to having a function as follows (although not quite the same since it's in a different context):

```
const onClick = () => {
  this.setState({clickCount: this.state.clickCount + 1})
}
```

Since the function is only one line, we can also remove one set of wrapping braces, and we end up with this:

```
{ onClick: () => this.setState({
    clickCount: this.state.clickCount + 1}) }
```

We covered how to place ClickCounter onto a page earlier in this section, something like this:

```
ReactDOM.render(e(ClickCounter),
  document.getElementById('react_container'));
```

The following screenshot shows the counter in the button when the page loads:

Furthermore, DjDt refers to the debug toolbar that we learned about in the Django Debug Toolbar section in Chapter 15, Django Third-Party Libraries:

![](img/4840a43efd335abd10b93708225692d6_715_0.png)

Figure 16.6 – Button with 0 for the count

After clicking the button a few times, the button looks as shown in *Figure 16.7*:

![](img/4840a43efd335abd10b93708225692d6_715_1.png)

Figure 16.7 – Button after clicking seven times

Now, just to demonstrate how *not* to write the render function, we’ll look at what happens if we just return HTML as a string, like this:

```
```javascript
render() {
    return '<button>' + this.state.clickCount + '</button>'
}
```
```

Now the rendered page looks as shown in *Figure 16.8*:

![](img/4840a43efd335abd10b93708225692d6_715_2.png)

Figure 16.8 – Returned HTML rendered as a string

This shows React’s automatic escaping of HTML in action. Now that we have had a brief intro to JavaScript and React, let’s add an example page to Bookr so you can see it in action.

## Exercise 16.01 – setting up a React example

In this exercise, we will create an example view and template to use with React. Then we will implement the ClickCounter component. At the end of the exercise, you will be able to interact with it with the ClickCounter button:

+   1. In PyCharm, go to New | File inside the project’s static directory. Name the new file react-example.js.

+   2. Putting this code inside it will define the React component, then render it into the react_container <div> that we will be creating:

```
const e = React.createElement;

class ClickCounter extends React.Component {
    constructor(props) {
        super(props);
        this.state = { clickCount: 0 };
    }

    render() {
        return e(
            'button', { onClick: () => this.setState({
                clickCount: this.state.clickCount + 1
            })}
            ,
            this.state.clickCount
        );
    }
}

ReactDOM.render(e(ClickCounter),
    document.getElementById('react_container'));
```

You can now save react-example.js.

+   3. Go to **New | HTML File** inside the project’s templates directory:

![](img/4840a43efd335abd10b93708225692d6_717_0.png)

Figure 16.9 – Create a new HTML file

+   4. Name the new file `react-example.html`:

![](img/4840a43efd335abd10b93708225692d6_717_1.png)

Figure 16.10 – Name the file react-example.html

You can change the title inside the `<title>` element to *React Example*, but that is not necessary for this exercise.

+   5. `react-example.html` is created with some HTML boilerplate as we have seen before. Add the following `<script>` tags to include React just before the closing `</body>` tag:

```
```html
<script crossorigin
src="https://unpkg.com/react@16/umd/react.development.js">
</script>
<script crossorigin src="https://unpkg.com/
react-dom@16/umd/react-dom.development.js"></script>
```
```

6. The react-example.js file will be included using a <script> tag, and we need to generate the script path using the static template tag. First, load the static template library at the start of the file by adding the following on the second line:

```
{% load static %}
```

The first few lines of your file will look like Figure 16.11:

```
<!DOCTYPE html>
{% load static %}
<html lang="en">
<head>
```

Figure 16.11 – The load static template tag included

Then, just before the closing </body> tag, but after the <script> tags that were added in step 5, add this script tag to include react-example.js:

```
<script src="{% static 'react-example.js' %}"></script>
```

7. We now need to add the containing <div> that React will render into. Add this element after the opening <body> tag:

```
<div id="react_container"></div>
```

You can save react-example.html.

8. Now we’ll add a view to render the template. Open the reviews app’s views.py file and add a react_example view at the end of the file:

```
def react_example(request):
    return render(request, "react-example.html")
```

In this simple view, we just render the react-example.html template with no context data.

9. Finally, we need to map a URL to the new view. Open the bookr package’s urls.py file. Add this map to the urlpatterns variable:

```
path('react-example/',
    reviews.views.react_example)
```

You can save and close urls.py.

10. Start the Django dev server if it’s not already running, then go to http://127.0.0.1:8000/react-example/. You should see the ClickCount button rendered as in Figure 16.12:

![](img/4840a43efd335abd10b93708225692d6_719_0.png)

Figure 16.12 – The ClickCount button

Try clicking the button a few times and watch the counter increment.

In this example, we created our first React component, then added a template and view to render it. We included the React framework source from a CDN. In the next section, we will introduce JavaScript XML (JSX), which combines templates and code into a single file that can simplify our code.

## JSX – a JavaScript syntax extension

It can be quite verbose to define each element using the React.createElement function – even when we alias to a shorter variable name. The verbosity is exacerbated when we start building larger components.

When using React, we can use JSX to build the HTML elements instead. JSX stands for JavaScript XML since both JavaScript and XML are written in the same file. For example, consider the following code in which we are creating a button using the render method:

```
return React.createElement('button', { onClick: ... },
  'Button Text')
```

Instead of this, we can return its HTML directly, as follows:

```
return <button onClick={...}>Button Text</button>; 
```

Note that the HTML is not quoted and returned as a string. That is, we are not doing this:

```
return '<button onClick={...}>Button Text</button>';
```

Since JSX is an unusual syntax (a combination of HTML and JavaScript in a single file), we need to include another JavaScript library before it can be used: Babel (https://babeljs.io). This is a library that can transpile code between different versions of JavaScript. You can write code using the latest syntax and have it transpiled (a combination of translate and compile) into a version of code that older browsers can understand.

Babel can be included with a <script> tag like this:

```
<script crossorigin
src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>
```

This should be included on the page after your other React-related script tags but before you include any files containing JSX.

Any JavaScript source code that includes JSX must have the type="text/babel" attribute added:

```
<script src="path/to/file.js" type="text/babel"></script>
```

This is so Babel knows to parse the file rather than just treat it as plain JavaScript.

> **Note**
> 
> Note that using Babel in this way can be slow for large projects. It is designed to be used as part of the build process in an npm project and to have your JSX files transpiled ahead of time (rather than in real time, as we are doing here). The npm project setup is beyond the scope of this book. For our purposes and with the small amount of JSX we are using, using Babel will be fine.

JSX uses braces to include JavaScript data inside HTML, similar to Django’s double braces in templates. JavaScript inside braces will be executed. We’ll now look at how to convert our button creation example to JSX. Our render method can be changed to this:

```
render() {
    return <button onClick={() =>this.setState({
        clickCount: this.state.clickCount + 1
    })>
        {this.state.clickCount}
    </button>;
}
```

Note that the onClick attribute has no quotes around its value; instead, it is wrapped in braces. This is passing the JavaScript function that is defined inline to the component. It will be available in that component’s props dictionary that is passed to the constructor method. For example, imagine that we had passed it like this:

```
onClick="() =>this.setState..."
```
