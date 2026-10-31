
```
code: [
    if x < n [
        print {x is smaller than n}
    ]
    print {in the middle of the code}
    probe "true"
    probe n
    if n = y [
        probe "n and y are equal"
    ]
]
```

We want to extract to a `result` series, all occurrences of a word followed by a string, such as `print {...}` or `probe "true"`. We can accomplish this with the following rule:

```
result: []
;-- if word-string match, append to result
rule: [ start: word! string! finish: 
      ( append result copy/part start finish )
]
```

Notice how `start` and `finish` are used to delimit the word/string combination. The following `parse` code does what we want:

```
parse code [
    some [
        rule          ; (1)
    |
        ahead block!  ; (2)
        into rule     ; (3)
    |
        skip          ; (4)
    ]
]
```

In line `(1)`, we try to match the word/string rule. When matched, the word and the string are appended to `result`. If there is no match, and we find a block `\[ \]` in line `(2)`, we go into it in line `(3)` and apply the rule. `ahead` looks for a match to the right, but does not advance the parse pointer.

If there was no word/string match and no block, we would simply `skip` to the next position with line `(4)`.

The resulting series `result` contains all occurrences in the code where a word was followed by a string—`probe result` shows us the following:

```
[
    print "x is smaller than n"
    print "in the middle of the code"
    probe "true" 
    probe "n and y are equal"
]
```

This is even executable code—`do result` gives us the following output:

```
x is smaller than n
in the middle of the code
"true"
"n and y are equal"
; == "n and y are equal"
```

# Debugging parse

Sometimes, you need some help to see how the `parse` process is working. A simple measure is to include a `print` or `probe` statement with certain matches, such as when parsing the `rate` string in the *Searching, extracting, and changing* section:

```
rate: "<title>1 USD = 0.81191502 EUR</title>"
parse rate [thru "<title>" copy data to "</title>" (probe data) thru ">"]
;"1 USD = 0.81191502 EUR"
;== true
```

Or you could simply print out:

`(print "found a ``</title>")`, to indicate that a match was found.

Let's apply this to the last example from the previous section:

```
rule: [start: word! string! finish:
    ( 
        print ["-->" start]
        print ["<--" finish]
        append result copy/part start finish 
    )
]
```

This prints out the following:

```
--> print x is smaller than n
 <--
 --> print in the middle of the code probe true probe n if n = y probe n and y are equal
 <-- probe true probe n if n = y probe n and y are equal
 --> probe true probe n if n = y probe n and y are equal
 <-- probe n if n = y probe n and y are equal
 --> probe n and y are equal
 <--
 ;== true
```

We can get a similar, but even more detailed output of each match by replacing `parse` with `parse-trace`:

```
parse-trace rate [thru "<title>" copy data to "</title>" (probe data) thru ">"]

-->
match: [thru "<title>" copy data to "</title>" (probe dat
input: "<title>1 USD = 0.81191502 EUR</title>"
-->
==> matched
<--
match: ["<title>" copy data to "</title>" (probe data) th
input: "1 USD = 0.81191502 EUR</title>"
-->
-->
==> matched
<--
<--
match: [data to "</title>" (probe data) thru ">"]
input: "</title>"
"1 USD = 0.81191502 EUR"
match: [to "</title>" (probe data) thru ">"]
input: "</title>"
-->
==> matched
<--
return: true
== true
```

```
 
```

Much more can be done with the `/trace` refinement:

```
parse/trace input rules callback
```

`callback` is a function that responds to events in the parsing process and can be used to provide tracing, statistical, and debugging information.

# Summary

Now you have learned to perform simple and the more complex tasks of recognizing, analyzing, and manipulating data by using `parse`. We first got to know the `bitset!` datatype and its handy charset equivalent. Then, we learned how to `parse` with `to` and `thru`, extract data (with `copy`), and change data. We saw how we can attach code to a `rule` to be executed when it matches. Finally, we explored how to `parse` blocks and code, and how to get a detailed output of how `parse` works in a specific case.

This chapter was not exhaustive, some other words exist in `parse` which we haven't talked about. In fact, the `parse` dialect deserves a book on its own.

# Questions

1. Make a `bitset` to store all lowercase letters and another for all uppercase letters.

2. Use `union` to make a `bitset` storing all alphanumerical characters.

3. Use `union` to make a `bitset` storing all characters used in hexadecimal numbers.

4. Make a rules pattern `integer` to match with integer numbers, starting optionally with a + or - sign.

5. Make a pattern that matches the following product codes: `"#ABC-nnnn"` or `"#XYZ-nnnn"`, where `nnnn` is a four-digit pattern.

6. Try for yourself to `parse` the `data` string to extract the exchange rate `0.81191502`. (Hint: use `some` and the `letter` charset.)

7. Starting with `rate: "<title>1 USD = 0.81191502 EUR</title>"`, change both tags to `<rate>` tags, by using two rules.

8. Use `parse` and the code within it to obtain the exchange rate info in this form: `\[ "EUR" ``0.81191502 "GPB" 0.70675954 ... \]`

9. Parse strings that contain two different letters (in random order and numbers). The `parse` must return true and print out each letter it finds.

10. Rewrite the following code so that it works with a block `\[x x x y y y\]`:

```
parse "xxxyyy" [copy letters some "x" (n: length? letters) n "y"] ;== true
letters        ;== "xxx"
```

# Composing Visual Interfaces

Red comes with a built-in, cross-platform, graphical capability called `View`. This is unique among current programming languages. For most of them this is not a priority, and when it is possible, it is tedious and leads to verbose code. Here, Red really shines. You'll be amazed by the elegance with which you can build complex interactive screens in a simple declarative way.

As of version 0.6.3, Windows and macOS are fully supported, the other platforms (Linux, Android, iOS) are partly ready and scheduled for the near future.

Red relies on *native widgets.* It binds on each platform to the default native GUI API available, so that users get to see familiar graphics. On Linux, `Visual Interface Dialect` (`VID`) will rely on the well-known GTK graphical toolkit.

We'll build further on several examples from the previous chapters, to download exchange rates and develop an interactive contact maintenance app.

The following topics will be covered in this chapter:

* Structuring an interface

* A faces walkthrough

* Events and actors

* Basic contacts app

* An introduction to the draw dialect

# Technical requirements

You'll find the code for this chapter at [https://github.com/PacktPublishing/Learn-Red-Fundamentals-of-Red/tree/master/Chapter09](https://github.com/PacktPublishing/Learn-Red-Fundamentals-of-Red/tree/master/Chapter09). If you have installed Red as indicated in [Chapter 2](#e7b0526e-a6dd-4c51-abc8-81b1dfdc8460.xhtml), *Setting Up for Development*, you are good to go. You can work on Windows, macOS, or Linux. You can type or paste any code in the Red console to see its results, or you can start using an editor, such as Visual Studio Code.

# Structuring an interface

`View`, the graphical interface engine, is completely written in Red and Red/System. The following diagram shows how `View` is structured with the help of three other parts in a bit more detail:

* `VID`: The `Visual Interface Dialect`

* `Draw`: The dialect to draw two-dimensional shapes

* `React`: This is an engine that makes the whole system automatically and fully reactive to the user or system events

You can check their relationship in the following screenshot:

![](assets/827c9190-782e-4486-a052-3ec0e4e15ded.png)
