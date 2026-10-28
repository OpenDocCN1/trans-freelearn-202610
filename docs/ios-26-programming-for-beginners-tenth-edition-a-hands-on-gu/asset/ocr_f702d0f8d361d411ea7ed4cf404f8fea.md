> For more information on loops, visit https://docs.swift.org/swift-book/documentation/the-swift-programming-language/controlflow.

Let’s look at each type in turn.

# for-In loops

The for-in loop steps through every value in a sequence, and a set of statements in curly braces, known as the loop body, is executed each time. Each value is assigned to a temporary variable in turn, and the temporary variable can be used within the loop body. This is what it looks like:

```
for item in sequence {
    code
}
```

The number of times the loop repeats is dictated by the number of items in the sequence. Let’s begin by creating a for-in loop to display all the numbers in myRange. Follow these steps:

+   1. Add the following code to your playground and run it:

```
for number in myRange {
    print(number)
}
```

Here, the statement inside the loop is executed 11 times since myRange includes the last number in the range. You should see each number in the sequence displayed in the Debug area, and the last value printed in the Debug area is 20.

+   2. Let’s try the same program, but this time with myRange2. Modify the code as follows and run it:

```
for number in myRange2 {
    print(number)
}
```

In this case, the statements inside the loop are executed 10 times, and the last value printed in the Debug area is 19.