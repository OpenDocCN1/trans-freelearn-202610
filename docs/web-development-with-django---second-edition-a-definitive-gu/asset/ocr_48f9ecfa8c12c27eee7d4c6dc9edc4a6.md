We can mix and match the method of passing the values too. This is also valid:

```
ReactDOM.render(<ClickCounter name="Ben" target={5}/>, document.getElementById('react_container'));
```

In the next exercise, we will update ClickCounter to read these values from properties and change its behavior when the target is reached. We will pass these values in from the Django template.

# Exercise 16.03 – React component properties

In this exercise, we will modify ClickCounter to read the values of target and name from its props. We will pass these in from the Django view and use the escapejs filter to make the name value safe for use in a JavaScript string. When you are finished, you will be able to click on the button until it reaches a target, and then see a Well done message. Let's get started with the steps:

+   1. In PyCharm, open the reviews app's views.py. We will modify the react_example view's render call to pass through a context containing name and target, like this:

```
return render(request, "react-example.html", {"name": "Ben", "target": 5})
```

You can use your own name and pick a different target value if you like. Save views.py.

+   2. Open the react-example.js file. We will update the state setting in the constructor method to set the name and target from props, like this:

```
constructor(props) {
  super(props);
  this.state = { clickCount: 0, name: props.name, target: props.target};
}
```

+   3. Change the behavior of the render method to return Well done, <name>! once target has been reached. Add this if statement inside the render method:

```
if (this.state.clickCount === this.state.target) {
  return <span>Well done, {this.state.name}!</span>;
}
```

+   4. To pass the values in, move the ReactDOM.render call into the template so that Django can render that piece of code. Cut this ReactDOM.render line from the end of react-example.js:

```
ReactDOM.render(<ClickCounter/>, document.getElementById('react_container'));
```

We will paste it into the template file in step 6. react-example.js should now only contain the ClickCounter class. Save and close the file.