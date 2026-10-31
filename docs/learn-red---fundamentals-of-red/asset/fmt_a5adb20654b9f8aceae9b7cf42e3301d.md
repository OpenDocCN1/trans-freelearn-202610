
```
if error? try [4 < "abc"][
    print "Wrong input, enter an integer!"
    quit ; or: continue
]
```

We could also use `either error? try`:

```
either error? try [4 < "abc"][
    print "Wrong input, enter an integer!"
    quit ; or: continue
 ][
    ; everything ok, now we can proceed
 ]
```

You can find out more details about an error that occurred with the `probe` word:

```
if error? err: try [4 < "abc"] [probe err]
```

This produces the following output, which is useful when debugging:

```
make error! [
    code: 339
    type: 'script
    id: 'invalid-compare
    arg1: 4
    arg2: "abc"
    arg3: none
    near: none
    where: '<
    stack: 77629472
]
*** Script Error: cannot compare 4 with "abc"
*** Where: <
*** Stack: probe
```

# Creating and catching errors

In a situation where you know a runtime error could occur, you can invoke a specific error yourself by using `cause-error`. For example:

```
m: 10
if n = 0 [ cause-error 'math 'zero-divide [] ]
print m / n
```

This line generates a `*** Math Error: attempt to divide by zero` when `n` is `0`. This could be useful when entering a function with an `n` parameter that could be `0`, and when we want to divide by `n` in the function.

Note that it needs the literal words `'math` and `'zero-divide`, describing the error's type and name; the third argument is a parameter block, which can be empty. Instead of `cause-error`, you can also encounter `make error! [math zero-divide]` or even `make error! 400`. They all do the same—generate the error with that name or code.

Only use `cause-error` if you want to stop your program displaying the error message.

So to summarize, use `try`, `if error?`, `cause-error`, and the `error!` object together for proper error handling in Red.

The `catch - throw` phrase is meant for controlling code flow, as we saw in the `Catch and throw` section of this chapter, not for error-handling.

# Checking types

Writing code that terminates with a runtime error is rather unprofessional, so whenever possible, use code that does not create an error. In our game, you could test the type of `guessed-number` with the following:

```
if integer? guessed-number 
```

If `guessed-number` was not an integer, use `continue` to ask for a next guess:

```
;-- see Chapter04/guess-number2.red:
if not integer? guessed-number [
    print ["This is no integer! Try again."]
    continue
]
```

You could also use the more complicated `if (type? guessed-number) <> integer!` here, but we strive to make things simpler in Red, remember?

To check whether a `value` is a number (integer or float), use `number? value`.

We'll come back to this subject in [Chapter 6](#5c76066d-61d4-4dbd-b5db-902ae9e9991c.xhtml), `Using Functions and Objects`, to discuss error handling with functions.

# Summary

We have learned a lot in this chapter! We now know how to get console input from the user. We have learned various ways to bend our code to respond to conditions with `if`, `either`, `switch`, `case` or `catch`, and `throw`. We also saw that repeating code can be made with `forever`, `loop`, `repeat`, `while`, and `until`. We discussed how to perform error handling with `attempt`, `try`, `error?`, and `cause-error`. Finally, we built a number-guessing game that makes use of most of these techniques.

By now, you should be able to write out a program's logic in simple scripts. In the following chapter, you will learn how to work with blocks and series, the foundational data structure in Red.

# Questions

1. Give the results of the following statements. Do it in your head first, then verify it in the console:

```
      if 0 [print "0 is true"]
      if 13 = 13.0 [print "ok"]
      if 13 == 13.0 [print "ok"]
```

2. What is the value of `out` after executing the following statements?

```
      p: 7
      out: either p == 7.0 ["p is 7"]["bad luck"]
```

3. Write a code snippet that asks the user whether it is raining, and provide the appropriate whether or not it is raining. Use the multiline block style.

4. Write a `switch` snippet where the values are of different datatypes—for example, use an `item` variable that has the `"red"` value and test on different values.

5. Write a `case` snippet that tests whether a `name` word contains a vowel (a, e, i, o, or u). Hint: The conditions are of the form `find name "a";`, and so on. Look up an explanation with `? find` in the REPL. We'll discuss `find` in the next chapter.

6. Write a loop that produces the output `1 2 3 4 5 6 7`. Hint: Use `append` and `form`, as in the `forever` loop.

7. Write a rocket countdown (from 10 to 1).

# Working with Series and Blocks

As you know by now, nearly everything in Red gets written inside `[]`. These denote blocks, which are a kind of series. Blocks and series form the foundation of Red code; in fact, if you look at the type hierarchy in [Chapter 3](#6609439d-2797-41d5-97e3-5b2efe61d4bb.xhtml), `Using Words, Values, and Types`, a block is also a `series!` type. A `series!` type contains a lot of other common types besides blocks, such as strings, files, binary values, tags, emails, images, and so on. That's why the functions and actions that work with block series can be applied to other types of values. For example, strings are essentially series of characters, and so the techniques used to manipulate block series can also be used for string operations.

In this chapter, we will learn how to work with series. We'll learn how to view a series as something that has a head and a tail, and a moving index. We'll learn how to get information from a series, how to move through it, search, sort, and change it. These operations are the bread and butter of a Red developer, and mastering the use of series is a core skill required for attaining Red proficiency!

In this chapter, we will cover the following topics:
* Series and blocks
* Navigating and looping through a series
* Getting information from a series
* Changing a series
* Copying a series
* Strings as series

## Technical requirements

You'll find the code for this chapter at [https://github.com/PacktPublishing/Learn-Red-Fundamentals-of-Red/tree/master/Chapter05](https://github.com/PacktPublishing/Learn-Red-Fundamentals-of-Red/tree/master/Chapter05). If you have installed Red on Windows, OS X, or Linux, as indicated in [Chapter 2](#e7b0526e-a6dd-4c51-abc8-81b1dfdc8460.xhtml), `Setting Up for Development`, you are good to go. You can type or paste any code in the Red console to see its results, or you can start using an editor, such as Visual Studio Code.

## Series and blocks

Let's look at an example. The letters `A B C D` are contained within the block `[A B C D]`. Coming from other programming languages, you may be more used to terms such as *lists* or *arrays*. These are very much the same as series. In Red, multidimensional arrays can be represented by nested blocks. If you want a more formal definition, we could say that a series is *an ordered set of items*, which can be values (data) or words (code). In the preceding example, the items are words such as `A B` and so on (they are not strings or characters), and they must get a value at some time while running the program. In Red, you can manipulate code as easily as you can manipulate data.

=> Now answer question 1 from the `Questions` section.

Because a series is ordered, each item in it can be referred to by a number—its `index`. The index in Red starts counting from `1`, so the item with index `3` in the preceding series is `C`. In the string series `"Good dog!"`, the index `4` item is the `#"d"` character. There is also the `empty series`, which has no elements—`[]` or `""`.
