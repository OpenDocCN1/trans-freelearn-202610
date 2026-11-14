
# ES2015 Features

## Destructuring

```
// Snippet Example 6b
function findElements(options) {
  var first = false,
      query,
      els;
  if (options !== undefined && typeof options === "object") {
    if (options.first !== undefined) {
      first = options.first;
    }
    if (options.query !== undefined) {
      query = options.query;
    }
  }
  els = [].slice.call(document.querySelectorAll(query));
  if (first === true) {
    els = [els[0]];
  }
  return els;
}
console.log(findElements({
  query: "a",
  first: true
}).map(function(o) {
    return o.textContent;
  }));
```

### Note

The snippets in this section are located at `snippets/02/ex6-destructuring/a`...`d` in the code package of this book. Using the interactive snippet playground, select `2: ES2015 and Browserify` in the first dropdown and examples `6a … 6d` in the second dropdown.

The first several lines are devoted to supplying appropriate defaults and extracting values out of the `options` object. This is largely boilerplate; but if it is incorrect, bugs can arise. It would be so much better if we could avoid this entirely.

In ES2015, we can extract these values easily using a new feature called `destructuring`. It lets us write code like the following:

```
// Snippet Example 6a
let {a, b} = {
  a: 5,
  b: 10	
};
console.log(a, b); // 5 10
```

We can do this safely, even if the object doesn't contain one of the keys we're asking for—the value will simply be `undefined`. Or, we can specify defaults:

```
let {a, b = 10} = {
  a: 5
}; // a = 5, b = 10
```

We can also drill into the object, as follows:

```
let {a: {b, c=10}} = {
  a: {
    b: 5
  }
};
console.log(b, c); // 5, 10, a is undefined
```

The same applies to arrays, as seen in the following snippet:

```
let [state, capitol] = ["Illinois", "Springfield"];
```

If there are more items than you care about, you can skip them:

```
let [a, , c] = [1, 2, 3];
console.log (a, c); // 1 3
```

You can use the `...` operator as well, should you wish to collect all the remaining values:

```
let [a, ...b] = [1, 2, 3];
console.log(a, b); // 1, [2, 3]

let {c, ...d} = {
  c: 5,
  d: 10,
  e: 15
}
console.log(c, d); // 5, {d: 10, e: 15}
```

Once we put all this together, we can rewrite our original code snippet as follows (in ES2015):

```
// Snippet Example 6c
function findElements({query, first = false}) {
  let els = [].slice.call(document.querySelectorAll(query));
  if (first === true) {
    els = [els[0]];
  }
  return els;
}
console.log(findElements({
  query: "a",
  first: true
}).map(o => o.textContent));
```

Although it may not be immediately obvious, this also gives us another feature: named parameters. It's not quite as nice as it is in other languages that support named parameters, but it's good enough. It also means that our code is even more self-documenting.

We can use this to approximate multiple return values as well:

```
// Snippet Example 6d
function doSomethingThatErrors() {
  let error = 5,
    message = "Element could not be found";
  return {
    error,
    message
  };
}
let {error, message} = doSomethingThatErrors();
console.log(error, message); // 5 Element could not be found
```

## String Interpolation

Many template and interpolation libraries have been developed for JavaScript, but ES2015 now provides built-in string interpolation.

### Note

The snippets in this section are located at `snippets/02/ex7-string-interpolation/a` in the code package of this book. Using the interactive snippet playground, select `2: ES2015 and Browserify` in the first dropdown and example `7` in the second dropdown.

If you recall, we have already written an interpolation method named `interpolate` in the `Variable Arguments` section. For example, `interpolate("My name is :1", "Bob")` would return `My name is Bob`. In ES2015, we can achieve the same thing with the following:

```
let name = "Bob",
    str = `My name is ${name}`;
```

For most interpolation libraries, this is where it stops. But ES2015 allows us to evaluate arbitrary expressions:

```
let x = 2,
    y = 4,
    str = `X=${x},Y=${y},X+Y=${x + y}`; // X=2,Y=4,X+Y=6
```

### Note

`Anything` that is an expression will work, including accessing object properties and methods.

As an added bonus, this new syntax also permits multiline strings:

```
let str = `SELECT * FROM
             FROM aTable`;
```

Of course, all whitespace is included, which means the string actually becomes `"SELECT *\n FROM aTable"`. If spacing is important, you would need to take the necessary steps to strip the whitespace.

You can also define your own `interpolations`, which can be very useful. Let's consider a common security problem—SQL injection. For example, some developers are a bit lax when it comes to how they construct their SQL statements, as in the following example:

```
var sql = "SELECT * FROM customers WHERE customer_name = \"" + name + "\"";
```

Unfortunately, it's possible to inject arbitrary SQL code into this statement by supplying a specially crafted customer name. Now, before you think that ES2015 interpolation has saved you from security issue, be careful. The following construct is just as insecure:

```
let sql = `SELECT * FROM customers WHERE customer_name = ${name}`;
```

### Tip

`Do not ever write code like the prior code! Ever!`

Instead, most libraries that work with SQL commands allow you to craft `prepared statements`. These statements are comprised of two parts: a SQL template and an array of variables that are slotted into their appropriate positions in the template. This sounds an awful lot like interpolation, and it is. The difference is that these libraries use the database's own code to properly handle values such that SQL injection becomes impossible.

It looks something like the following in practice:

```
var preparedStatement = {
  sql = "SELECT * FROM customers WHERE customer_name = ?",
  binds: ["Bob"]
};
```

It's not terribly difficult to write SQL statements this way; but with ES2015, we `can` write our own interpolation handler that will make writing prepared statements even easier:

```
function sql(s, ...binds) {
  let len = binds.length;
  return {
    sql: s.map((val, idx) => (`${val}${idx < len ? "?" : ""}`))
      .join(""),
    binds
  };
}

let name = "Bob";
let preparedStatement = /*[1]*/ sql`SELECT * FROM customers WHERE customer_name = ${name}`;
console.log(JSON.stringify(preparedStatement, null, 2));
// Console outputs
// {
//   sql: SELECT * FROM customers WHERE customer_name= ?
//   binds: ["Bob"]
// }
```

Take a look at `[1]` in the prior code. This is ES2015's way of indicating that the `sql` method is to be called in order to interpolate the string. Internally, this converts to `sql(["SELECT * FROM CUSTOMERS W` `HERE customer_name = ", ""], ["Bob"]);`.

## Promises and a Taste of ES2016

One often needs to write asynchronous code in JavaScript, `especially` when using Cordova plugins and also if one needs to make an `XMLHttpRequest`. This means that you will typically end up writing code as follows:

```
// Snippet Example 8a
function slowDiv(a, b, callback) {
  setTimeout(function() {
    if (b === 0) {
      callback("Can't divide by zero");
    } else {
      callback(undefined, a / b);
    }
  }, 2500 + Math.random() * 10000);
}
slowDiv(10, 5, function(err, result) {
  if (err !== undefined) {
    console.log(err);
    return;
  }
  console.log(result);
});
```

### Note

The snippets in this section are located at `snippets/02/ex8-promises/a` and `snippets/02/ex8-promises/d` in the code package of this book. Using the interactive snippet playground, select `2: ES2015 and Browserify` in the first dropdown and examples `8a-8d` in the second dropdown.

The previous example implements a very simple asynchronous method called `slowDiv` which it returns the results of a division operation after a short random period of time. If the divisor is zero, an error is returned instead.
