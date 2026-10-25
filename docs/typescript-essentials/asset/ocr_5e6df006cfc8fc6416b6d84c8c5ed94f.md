```
    return x + y;
}
```

As you can see, we now have two declarations of the add function: one for strings, one for numbers, and then we have the final implementation using the any type. The signature of the actual function implementation is not included in the function’s type definition, though. Attempting to call our add method with anything other than a number or string will fail at compile time, however, the overloads have no effect on the generated JavaScript. All of the type annotations are stripped out, as well as the overloads, and all we are left with is a very simple JavaScript method:

```
function add(x, y) {
    return x + y;
}
```

Great, so now we have a multipurpose add function that can take two values and combine them together for either strings or numbers. This still feels a little limited in overall functionality though. What if we wanted to add an indeterminate number of values together? We would have to call our add method over and over again until we eventually had only one value. Thankfully, TypeScript includes rest parameters, which is essentially an unbounded list of optional parameters.

The following code shows how to modify our add functions to include a rest parameter:

```
function add(arg1: string, ...args: string[]): string;
function add(arg1: number, ...args: number[]): number;
function add(arg1: any, ...args: any[]): any {
    var total = arg1;
    for (var i = 0; i < args.length; i++) {
        total += args[i];
    }
    return total;
}
```

A rest parameter can only be the final parameter in a function’s declaration. The TypeScript compiler recognizes the syntax of this final parameter and generates an extra bit of JavaScript to generate a shifted array from the JavaScript arguments object that is available to code inside of a function. The resulting JavaScript code shows the loop that the compiler has added to create the array that represents our indeterminate list of parameters:

```
function add(arg1) {
    var args = [];
    for (var _i = 0; _i < (arguments.length - 1); _i++) {
        args[_i] = arguments[_i + 1];
    }
    var total = arg1;
    for (var i = 0; i < args.length; i++) {
        total += args[i];
    }
    return total;
}
```