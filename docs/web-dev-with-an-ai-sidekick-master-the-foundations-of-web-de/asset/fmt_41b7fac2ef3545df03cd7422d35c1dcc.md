
# Indentation

The most important rule to understand first is **indentation**. In Python, indentation is not cosmetic. It defines structure. Blocks of code that belong together are indented by the same amount, usually four spaces. There are no braces to mark where a block starts or ends. When the indentation level increases, you are inside a block. When it decreases, you have left that block.

For example, indentation determines if a statement is inside a block or not, as shown in the following code:

```
if True:
    print("This line is inside the block")
    print("So is this one")
print("This line is outside the block")
```

The colon at the end of the `if` line tells Python that a block follows. The indentation tells Python which lines belong to that block. Mixing tabs and spaces is a common beginner mistake.

**Good practice**: The rule to adopt early is simple: always use spaces and always use the same number of spaces. For most code examples for languages like JavaScript or C#, I use two spaces to save horizontal space in my print books. In this book's Python examples, I use the traditional four spaces because indentation matters so much.

## Python Comments

Python supports **comments**, which are notes for humans that Python ignores. Comments start with a `#`, as shown in the following code:

```
# This is a comment
count = 10  # This comment is inline
```

Comments are useful for explaining why something is done, not what the code already clearly says. If the code is hard to explain without comments, that is often a sign the code itself should be simplified.

## Python Statements and Variables

Python programs are made up of **statements**. A statement is a complete instruction that Python can execute. Some statements perform actions, like calling a function. Others define things, like variables or functions. Unlike some languages, Python usually uses one statement per line, which makes code easier to read and comprehend.

Next come **variables**. A variable is a name that refers to a value. In Python, you do not declare a variable's type explicitly. The type is inferred from the value you assign, as shown in the following code:

```
age = 42
name = "Mark"
pi = 3.14159
```

In the preceding code, `age` refers to an integer, `name` refers to a string, and `pi` refers to a floating-point number. Like JavaScript, Python is **dynamically typed**, which means the same variable name can later refer to a different type of value. That flexibility is useful, but it makes clear names and consistent use more important.

> **Prompt**: Please explain dynamic typing in Python with examples of both its strengths and pitfalls.

## Python Data Types

Python has a small set of built-in basic types that you will use constantly. The most common ones are integers (`int`), floating-point numbers (`float`), strings (`str`), and booleans (`bool`), as shown in the following code:

```
count = 10        # int
price = 9.99      # float
title = "Python"  # str
is_active = True  # bool
```

Strings in Python are enclosed in either single quotes or double quotes. Both are equivalent. Choose one style and be consistent. Strings can be combined using the `+` operator and repeated using `*`, as shown in the following code:

```
greeting = "Hello"
name = "World"
message = greeting + " " + name
print(message * 3)       # prints Hello WorldHello WorldHello World
```

## Python Expressions

Python supports **expressions**, which are combinations of values and operators that produce a result. Arithmetic expressions behave mostly as you would expect, as shown in the following code:

```
total = 10 + 3
difference = 10 - 3
product = 10 * 3
quotient = 10 / 3
modulus = 10 % 3
print(total, difference, product, quotient, modulus)
```

The `/` operator always produces a floating-point result. If you want integer division, use `//`, as shown in the following code:

```
result = 7 // 2   # result is 3
```

## Python Comparisons

**Comparisons** produce Boolean values. These are used heavily in decision-making, as shown in the following code:

```
x = 5
x > 3      # True
x == 5     # True
x != 10    # True
```

Note that `==` checks for equality. A single `=` is used only for assignment. Confusing these is a classic beginner error.

## Conditional Statements

To make decisions, Python uses **conditional statements**, most commonly `if`, `elif`, and `else`, as shown in the following code:

```
temperature = 18
if temperature > 20:
    print("It's warm")
elif temperature > 10:
    print("It's mild")
else:
    print("It's cold")
```

Only one branch runs. The first condition that evaluates to `True` wins. This structure reads top to bottom and closely mirrors how people explain decisions in plain language.

### Comparison and Logical Operators

Conditions are built using comparison operators like:

- `==` equal to
- `!=` not equal to
- `<`, `<=`, `>`, `>=`

Logical operators combine conditions:

- `and` requires both conditions to be `true`
- `or` requires at least one condition to be `true`
- `not` negates a condition

```
if rating >= 8 and rating <= 10:
    print("Positive")
```

### Truthiness in Python

Python allows non-boolean values to be used in conditions. This is known as truthiness. For example:

- Empty lists, empty strings, and zero evaluate to `false`
- Non-empty collections and non-zero numbers evaluate to `true`
- `None` evaluates to `false`

```
if titles:
    print("There are titles to process")
```

This checks whether the list named `titles` is non-empty, without explicitly comparing lengths.

### Structural Pattern Matching with match

Since Python 3.10, Python has **structural pattern matching**, introduced with the `match` statement. This fills the same role as a `switch` statement in JavaScript, but it is more flexible.

For example:

```
status = 404
match status:
    case 200:
        print("OK")
    case 404:
        print("Not found")
    case 500:
        print("Server error")
    case _:
        print("Unknown status")
```

In this simplest form, `match` behaves very much like a traditional `switch`. The underscore (`_`) is the default case.

Python's `match` is more flexible than a simple `switch` because it is based on pattern matching, not just value matching. It can match literal values, data structures, object attributes, type patterns, and combinations of patterns with guards. For example:

```
command = ("play", "Arrival")
match command:
    case ("play", title):
        print(f"Playing {title}")
    case ("stop", _):
        print("Stopping playback")
```

This is more flexible than a classic `switch` statement.

`("play", "Arrival")` is an example of a tuple. You will learn about tuples in the next section, *Data Structures*.

Now let's move on to looping.

## Looping Statements

**Looping statements** let a Python program repeat work efficiently instead of writing the same code over and over. Python's looping model is simple, but it is built around a simple idea: you usually loop over things, not over numeric indexes.

### The for Loop

The `for` loop is the most commonly used loop in Python. It iterates over an iterable, like a list, tuple, string, dictionary, `range`, or any object that can produce values one at a time:

```
titles = ["Arrival", "The Matrix", "Star Trek"]
for title in titles:
    print(title)
```
