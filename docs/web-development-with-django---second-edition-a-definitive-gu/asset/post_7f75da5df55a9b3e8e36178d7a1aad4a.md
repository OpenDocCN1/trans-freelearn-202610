
The final approach is progressive enhancement, which is (as mentioned) what we will be using. In this way, Django is still generating the HTML templates, and React sits on top of this to add interactivity:

![](img/4840a43efd335abd10b93708225692d6_704_0.png)

Figure 16.4 – HTML generated with Django with React providing progressive enhancement

Note that it is common to use multiple techniques together. For example, Django may generate the initial HTML to which React is applied in the browser. The browser can then query Django for JSON data to be rendered using React.

To gain an understanding of these approaches, we will first take a brief look at how we can use JavaScript in our web applications.

## An introduction to JavaScript

In this section, we will briefly introduce some basic JavaScript concepts. This includes data structures such as variables, constants, arrays, and objects and callables such as functions, classes, and methods. Different operators will be covered as we introduce them.

This will give us the requisite knowledge to begin a brief overview of the React framework.

## Loading JavaScript

JavaScript can either be inline in an HTML page or included in a separate JavaScript file. Both methods use the <script> tag. With inline JavaScript, the JavaScript code is written directly inside the <script> tags in an HTML file; for example, like this:

```
<script>
// comments in JavaScript can start with //
/* Block comments are also supported. This comment is
   multiple lines and doesn't end until we use a star then
   slash:/
let a = 5; // declare the variable a, and set its value to 5
console.log(a); // print a (5) to the browser console
</script>
```

Note that the console.log function prints out data to the browser console that is visible in Developer Tools of your browser:

![](img/4840a43efd335abd10b93708225692d6_705_0.png)

Figure 16.5 – The result of the console.log(a) call – 5 is printed to the browser console

We could also put the code into its own file (we would not include the <script> tags in the standalone file). We then load it into the page using the <script> tag’s src attribute, as we saw in Chapter 5, Serving Static Files:

```
<script src="{% static 'file.js' %}"></script>
```

The source code, whether inline or included, will be executed as soon as the browser loads the <script> tag.

## Variables and constants

Unlike in Python, variables in JavaScript must be declared using either the var, let, or const keywords:

```
var a = 1; // variable a has the numeric value 1
let b = 'a'; // variable b has the string value 'a'
const pi = 3.14; // assigned as a constant and can't be redefined
```

Just like in Python, though, a type for a variable does not need to be declared. You will notice that the lines end with semicolons. JavaScript does not require lines to be terminated with semicolons – they are optional. However, some style guides enforce their use. You should try to stick with a single convention for any project.

You should use the let keyword to declare a variable. Variable declarations are scoped. For example, a variable declared with let inside a for loop will not be defined outside the loop. In this example, we’ll loop through and sum the multiples of 10 to 90 and then print the result to console.log. You’ll notice we can access variables declared at the function level inside the for loop, but not the other way around:

```
let total = 0;
for (let i=0; i< 10; i++) { // variable i is scoped to the loop
    let toAdd = i * 10; // variable toAdd is also scoped
total += toAdd; /* we can access total since it's in
the outer scope */
```

```
}
console.log(total); // prints 450
console.log(toAdd); /* throws an exception as the variable is not declared in the outer scope */
console.log(i); /* this code is not executed since an exception was thrown the line before, but it would also generate the same exception */
```

const is for constant data and cannot be redefined. That does not mean that the object it points to cannot be changed, though. For example, you couldn't do this:

```
const pi = 3.1416;
pi = 3.1; /* raises exception since const values can't be reassigned */
```

The var keyword is required by older browsers that don't support let or const. Only 1% of browsers these days don't support those keywords, so throughout the rest of the chapter, we will only use let or const. Like let, variables declared with var can be reassigned; however, they are scoped at the function level only.

JavaScript supports several different types of variables, including strings, arrays, objects (which are like dictionaries), and numbers. We will cover arrays and objects in their own sections now.

## Arrays

Arrays are defined similarly to how they are in Python, with square brackets. They can contain different types of data, just like with Python:

```
const myThings = [1, 'foo', 4.5];
```

Another thing to remember with the use of const is that it prevents reassigning the constant but does not prevent changing the variable or object being pointed to. For example, we would not be allowed to do this:

```
myThings = [1, 'foo', 4.5, 'another value'];
```

However, you could update the contents of the myThings array by using the push method (like Python's list.append) to append a new item:

```
myThings.push('another value');
```

## Objects

JavaScript objects are like Python dictionaries, providing a key-value store. The syntax to declare them is similar as well:

```
const o = {foo: 'bar', baz: 4};
```

Note that, unlike Python, JavaScript object/dictionary keys do not need to be quoted when creating them – unless they contain special characters (spaces, dashes, dots, and more).

The values from o can be accessed either with item access or attribute access:

```
o.foo; // 'bar'
o['baz']; // 4
```

Also note that since o was declared as a constant, we cannot reassign it, but we can alter the object's attributes:

```
o.anotherKey = 'another value'  // this is allowed
```

## Functions

There are a few different ways to define functions in JavaScript. We will look at three. You can define them using the function keyword:

```
function myFunc(a, b, c) {
    if (a == b)
        return c;
    else if (a > b)
        return 0;
    return 1;
}
```

All arguments to a function are optional in JavaScript; that is, you could call the preceding function like this: myFunc(), and no error would be raised (at least during call time). The a, b, and c variables would all be the undefined special type. This would cause issues in the logic of the function. The undefined special type is kind of like None in Python – although JavaScript also has null, which is more similar to None. Functions can also be defined by assigning them to a variable (or constant):

```
const myFunc = function(a, b, c) {
    // function body is implemented the same as above
}
```

We can also define functions using an arrow syntax. For example, we can also define myFunc like this:

```
const myFunc = (a, b, c) => {
    // function body as above
}
```

This is more common when defining functions as part of an object, for example:

```
const o = {
myFunc: (a, b, c) => {
    // function body
}
}
```

In this case, it would be called like this:

```
o.myFunc(3, 4, 5);
```

We will return to the reasons for using arrow functions after introducing classes.

## Classes and methods

Classes are defined with the class keyword. Inside a class definition, methods are defined without the function keyword. The JavaScript interpreter can recognize the syntax and tell that it is a method. Here is an example class, which takes a number to add (through toAdd) when instantiated. That number will be added to whatever is passed to the add method, and the result returned:

```
class Adder {
    // A class to add a certain value to any number

    // this is like Python's init method
    constructor (toAdd) {
        /* "this" is like "self" in Python
           it's implicit and not manually passed into every
           method */
        this.toAdd = toAdd;
    }

    add (n) {
        // add our instance's value to the passed in number
        return this.toAdd + n;
    }
}
```
