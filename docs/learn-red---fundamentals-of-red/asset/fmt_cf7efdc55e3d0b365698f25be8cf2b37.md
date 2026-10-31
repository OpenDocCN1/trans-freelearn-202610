
Congratulations! You are now able to work with the fundamental data structure of Red. You have learned the concept of a series in Red, with its `head`, `tail`, and `next` words that are used to move the index position in the series. We have seen how to select an item with `pick` or the `/` syntax, and how to search for an item with `find` and `select`, as well as how to change, add, delete, and move items. Finally, we saw how to `copy` the value of a series, and how to use the series actions on strings.

The number of words to use for working with series may seem a bit overwhelming at first, but they are frequently used in code, so they will become familiar soon. It will also become clear that combining these words can result in some powerful code.

# Questions

1. How can you make the series `data: [A B C D]` into a series of characters? Is it by phrasing it as `data: ["A" "B" "C" "D"]`?

2. We now have a `data` series that contains `[B C D]`. Move to the tail by applying `next` three times, while binding `data` to the result. Get the info at each next stage with `head?`, `tail?`, and `index?`. Draw a schema.

3. Using the series from the end of the previous question, apply `back` with binding and get the info.

4. Find out what happens when we use `data: skip data -2`.

5. Given the series `[323 2498 94321 31 82`, print out the numbers bigger than 1,000.

6. What is the result when you select an item from an empty series?

7. Given the series `s: ["A" "B" [42.1 42.2 42.3] red]`, how can you get to item `42.2` with the path notation? Try the same with `pick`.

8. Given the `s` series from the previous question, what is the result of `length? find s "B"`?

9. Given the `ser` series with the value of `[[3 7][13 42][108 666]]`, what is the value of `ser/1/2`? How do you select item `108`? How do you change `666` to `999`?

10. What is the result of the following two expressions:

```
select/part ["Red" "Crystal" "Ruby" "Java" "Go" "Rust"] ["Java"] 4
find/part ["Red" "Crystal" "Ruby" "Java" "Go" "Rust"] ["Java"] 4
```

What happens if you change `4` to `3`?

11. Use `move` to move items from the head of a series to its tail. First, move one item, and then move a few items with `/part`.

12. Given the series `names: ["John" "Dave" "Jane" "Bob" "Sue"]`, write a code snippet that loops through the series and prints out the names that contain the character `a`. Then look for both the letters `a` and `e`.

# Using Functions and Objects

This chapter is about modularizing code through the use of functions and objects.

We have already encountered a lot of built-in functions, such as `print` and `cd`, but of course you can write user-defined functions as well. When a program's code gets longer and more complex, you want to be able to give a name to certain code segments. For example, when processing a data stream from a file or the network, you'll want to have a `process-request` or `process-record` function that contains the logic of dealing with one chunk of data. That way, you can call this function each time you need it. This leads to less code that is better structured and more readable. Less code means fewer bugs! Functions can take parameters, local variables, and refinements, and we'll be exploring how to use these later on in the chapter.

Another way to group code is to define an object. Red is not class-based, like most object-oriented languages in common use today, such as Java, Python, C#, or Ruby, it takes a more prototype-based approach, where new objects can be created on the basis of an already existing object—its prototype. In this chapter, we will explore what you can do with objects in Red in depth.

In this chapter, we will cover the following topics:

* A fauna of functions
* Function attributes
* Working with functions
* Code is data and data is code
* Using objects

## Technical requirements

You'll find the code for this chapter at `https://github.com/PacktPublishing/Learn-Red-Fundamentals-of-Red/tree/master/Chapter06`. If you have installed Red as indicated in `Chapter 2`, `Setting Up for Development`, you are good to go. You can work on Windows, OS X, or Linux. You can type or paste any code in the Red console to see its results, or you can use an editor, such as Visual Studio Code.

## A fauna of functions

Red's built-in functions can be categorized as follows. The type is indicated within `()`:

* `operators` (`op!`), which can be used with infix notation, such as `a + b`, `a / b`, and so on.
* `native` (`native!`) functions, such as `if`, `either`, `while`, `throw`, `all`, `wait`, and so on; you can see a complete list by using `? op!` in the console.
* `routine` (`routine!`) functions, such as `exists?`, `write-clipboard`, and so on; you can see a list using `? routine!` It is also used in Red to define a function that calls a Red/System function (see `Chapter 10`, `Advanced Red`).
* `action` (`action!`) functions, such as `copy`, `move`, `clear`, `to`, `form`, and so on; you can see a complete list using `? action!`

The native, routine, and action functions are written in Red/System. Action functions are special in that they are polymorphic—they are defined for more than one datatype. Which action code is executed depends on the type of its first argument.

* `mezzanine` (`function!`) functions, which are higher-level functions written in Red; you can see a complete list using `? function!`

In this section, we'll cover the variety of ways that exist to define your own functions.

### The do word

We will start by giving a name (a label) to a code block so that you can call that code by its name and evaluate it with the `do` word, which we already saw in action in the `Evaluation with do and reduce` section in `Chapter 3`, `Using Words, Values, and Types`. Here, the word `pri5` is bound to a piece of code, which prints five digits separated by dashes:

```
;-- see Chapter06/do-does-has-func.red:
pri5: [ repeat i 5 [prin i prin "-"] ]
do pri5 ;== 1-2-3-4-5- ; evaluate code block with label pri5
type? pri5 ;== block!
```

However, `pri5` is not yet a real function; it is still a block, and you have to invoke it with `do`.

The block could also contain interactive code, such as that shown in the following example:

```
do [
    pass: ask "Enter Password: "
    either pass = "secret" [
        print "Welcome back."
    ][
        print "Incorrect password."
    ]
]
```

We could even ask the user to interactively enter code while the program is running, and then execute that code to obtain a simple form of metaprogramming, as shown in the following code:

```
do [
    code: ask "Enter some code: "
    do append {print "Here's your running code..."} code
]
```

This could give us an output such as the following:

```
Enter some code:
loop 3 [prin "Hi!"] ;<-- input from user to ask
Here's your running code...
Hi!Hi!Hi!
```

Building further on this example, we can make a very primitive REPL with one line of code:

```
;-- see Chapter06/processor.red:
; #include %../../../red-source/environment/console/CLI/input.red
forever [print do ask "=> "]
```

Here is a possible user interaction:

![](assets/51860a35-ca53-47c4-bf94-e446a8dfa683.png)

If you compile, remember to uncomment the `#include` line and adapt the path to `input.red` on your machine. This starts to show what is possible with metaprogramming in Red!

Of course, any errors in the code entered interactively will only become apparent when the program runs.

=> Now answer question 1 from the `Questions` section.

### The does word

The simplest way to make your own function in Red is with the `does` word. Consider the following code:
