
# Red Functions – Typing, Return Values and Error Handling

## Optional Typing

This dynamic typing, which you perhaps know from Python, Ruby, or JavaScript, makes functions very flexible and general, but also vulnerable to these runtime errors.

In Red, typing is optional, which means you can work this way, but you can also indicate an argument's type as shown in the following screenshot:

```
inc: func [n [integer!]][n + 1]
```

Here we see you can restrict the type of the argument `n` to `integer!`. The reason could simply be that you don't need a general function. In this example, this forces the arguments to be of a certain datatype:

* `inc 3.14` gives the *** Script Error: inc does not allow float! for its n argument error
* `inc "abc"` returns the *** Script Error: inc does not allow string! for its n argument error

This tells us what is wrong with the function call when testing.

Developers who want to use your function now also see that an integer parameter is required here, so this is a form of implicit documentation.

Typing arguments also allows the compiler to produce much more efficient native code.

In our previous example, we could have typed the function as follows:

```
passing-ref: function [ data [block!] ] [ ... ]
```

You can specify multiple types, like the following example:

```
inc: func [n [integer! float!]][n + 1]
inc 3.14     ;== 4.14
```

You can also specify a supertype (`typeset!`) of these types:

```
inc: func [n [number!]][n + 1]
```

=> Now answer question 5 from the *Questions* section.

## Return values – return and exit

When the code in a function has been executed, control returns to the line after the function was called and a value is returned. This value is simply the last value computed in the function, such as the value `n + 1` in the `inc` example.

If you want to return a value earlier, you can do so with the `return` word. Control then also returns to where the function was called. Look at the following example, which is a `search-val` function that searches for a value in a given series. It returns the match and the rest of the series when found with `return series`; if no match is found, the `none` value is returned as the last value of the function:

```
search-val: func [series value] [
    forall series [
        if (first series) = value [
            return series
        ]
    ]
    none
]

search-val [10 20 30 40] 30    ;== [30 40]
search-val [10 20 30 40] 9     ;== none 
```

To return multiple values from a function, return a block that has been reduced, such as the one in the following version of `search-val`:

```
search-val: func [series value] [
    forall series [
        if (first series) = value [
            return reduce [series index? series]
        ]
    ]
    none
]

search-val [10 20 30 40] 30         ;== [[30 40] 3]
```

You can also indicate that the return value must be of a certain type, as shown in the following example where we affirm that the `double` function returns an `integer` value:

```
double: function [
    n [integer!] 
    return: [integer!]
][
    2 * n 
]
```

If you want to leave a function without returning a value, use the `exit` word.

For readability, it is better that the contents of the first block get split on to several lines when indicating the types. This becomes even more natural when adding refinements and documentation strings.

## Refinements

User-defined functions can also have refinements, which are like switches that can be optionally specified when calling a function. In the following example, we define a `sum` function with a `/avg` refinement:

```
sum: func [arg1 arg2 /avg][
    either avg [arg1 + arg2 / 2][arg1 + arg2]
]
sum 3 5       ;== 8
sum/avg 3 5   ;== 4
```

A refinement is tested as a Boolean value with the same name, but without the `/`. Also, don't leave a space between the function name and the refinement when calling `sum/avg`.

A refinement can also have a value, as shown in the following example:

```
div-sum: func [arg1 arg2 /div n][
    either div [arg1 + arg2 / n][arg1 + arg2]
]

div-sum 3 5             ;== 8
div-sum/div 3 5 4       ;== 2
```

Note that the refinement value follows the parameters. Better not overuse this, because it affects readability.

=> Now answer questions 6 and 7 from the *Questions* section.

## Code style and documentation strings

When a function starts to get more complex, incorporating arguments, refinements, return values, and so on, its code must stay readable, following a standard layout. Also, when used in production, functions and all their parts should have documentation. We can provide this with `docstrings`.

Here is how a general function should be styled:

```
func-name: func [
        "Comments for the function itself"
        arg1    [type1!] "Describe 1st argument"
        arg2    [type2!] "Describe 2nd argument"
        /ref1            "Describe refinement"
                argref1 [type1] 
        return: [type3!] "Describe return value" 
][
     ; function body
]
```

Here is a fully documented function in the standard layout:

```
max: function [
        "Return the maximum of 2 numbers"
        arg1 [number!] "First number argument"
        arg2 [number!] "Second number argument"
][
        either arg1 > arg2 [
            arg1
        ][
            arg2
        ]
]
max 42 7    ;== 42
```

When you ask `help max` or simply `? max`, these documentation comments are shown in the REPL:

```
>> help max
 USAGE:
 MAX arg1 arg2
 
 DESCRIPTION:
 Return the maximum of 2 numbers.
 MAX is a function! value.
 
 ARGUMENTS:
 arg1 [number!] "First number argument".
 arg2 [number!] "Second number argument".
```

## Working with functions

In this section, we will talk about how to make functions resistant to possible errors.

### Error handling

In the previous section, we saw that passing the wrong type of argument to a function causes the program to terminate with an error message. Is there a way to guard functions against wrong types and still let the program continue? We know how to test types with `type?`, so we can do that instead. Here is a version of the `inc` function that is untyped, but protected against the possibility of `n` not being an integer:

```
inc: func [n][
    if not integer? n [ 
        print ["n must be an integer, not a" (type? n)]
        exit
    ]
    n + 1
]

inc 9      ;== 10
inc pi     ;== n must be an integer, not a float
inc "abc"  ;== n must be an integer, not a string
```

Instead of the `if not type? n` test, we can also make this somewhat more readable:

```
inc: func [n][ 
    unless integer? n [ 
        print ["n must be an integer, not a " (type? n)]
        exit
    ]
    n + 1
]
```

This way, we can protect the start of a function with a number of tests, ensuring that, when the main logic starts, the input is reliable:

```
guarded-func: function [arguments][
    unless condA [ print "error-message" exit]
    unless condB [ print "error-message" exit]
    ; main logic
]
```

Red has some handy functions available to do this kind of protective test. The following are some examples:

* `zero?` returns `true` if the value is zero (`0`, `0.0`, `#"0"`, `0x0`).
* `none?` returns `true` if the value is `none`.
* `positive?` and `negative?` return `true` if the number value is > than `0` or < than `0`.
* `integer?`, `float?`, and `number?` are used to test the type of a number. In fact, this type of test is available for all types and built-in typesets!

Using these, we could simplify our previous guard statements to `if not integer? n` or `unless integer? n`.

Only if the guard cannot be expressed as an `if` test should you test dangerous code with `if error? try`.

Applying this to our `inc` example, this would become the following:

```
inc: func [n /local result][ 
    if error? try [result: n + 1][
        print ["n must be an integer, not a " (type? n)]
        exit 
    ]
    result
]

inc 9       ;== 10
inc "Red"   ;n must be an integer, not a string
```
