Here is the result of the preceding code:

```
Result is 3
```

We have two functions and the array **operations** in our example that stores references to them.

# Closures

A function can be created in the global scope or within the scope of another function. A function that can be referenced with an access to the variables in its lexical scope is called a closure, as shown in the following code:

```
library function_closure;

// Function returns closure function.
calculate(base) {
  // Counter store
  var count = 1;
  // Inner function - closure
  return () => print("Value is ${base + count++}");
}

void main() {
  // The outer function returns inner
  var f = calculate(2);
  // Now we call closure
  f();
  f();
}
```

Here is the result of the preceding code:

```
Value is 3
Value is 4
```

We have the `calculate` function, which contains the `count` variable and returns a an inner function. The inner function has an access to the `count` variable because both are defined in the same scope. The `count` variable exists only within the scope of `calculate` and would normally disappear when the function exits. This does not happen in this case because the inner function returned by `calculate` holds a reference to `count`. The variable has been closed covered, meaning it's within a closure.

Finally, we know what a **first-class function** is, where we can use them, and how important it is to use closures. Let's move ahead to classes and mixins.