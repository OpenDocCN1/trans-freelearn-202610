
## if statements

An if statement executes a block of code if a condition returns true, and optionally, another block of code if the condition returns false. It looks like this:

```
if condition {
    code1
} else {
    code2
}
```

Let’s implement an if statement now to see this in action. Imagine that you’re programming an app for a restaurant. The app would allow you to check if a restaurant is open, search for a restaurant, and check to see if a customer is over the minimum drinking age. Follow these steps:

- 1. To check if a restaurant is open, add the following code to your playground. Run it to create a constant and execute a statement if the constant’s value is true:

```
let isRestaurantOpen = true
if isRestaurantOpen {
    print("Restaurant is open.")
}
```

First, you created a constant, isRestaurantOpen, and assigned true to it. Next, you have an if statement that checks if the value stored in isRestaurantOpen is equal to true. Since the value is equal to true, the print() statement is executed and Restaurant is open is printed in the Debug area.

- 2. Try changing the value of isRestaurantOpen to false and running your code again. As the condition now returns false, nothing will be printed in the Debug area.

- 3. You can also execute statements if a value is false. Let’s say the customer has searched for a particular restaurant that is not in the app’s database, so the app should display a message saying that the restaurant was not found. Type in the following code to create a constant and execute a statement if the constant’s value is false:

```
let isRestaurantFound = false
if isRestaurantFound == false {
    print("Restaurant was not found")
}
```

2. Here’s how to write the if statement shown earlier as a switch statement. Type in the following code and run it:

```
trafficLightColor = "Yellow"
switch trafficLightColor {
case "Red":
    print("Stop")
case "Yellow":
    print("Caution")
case "Green":
    print("Go")
default:
    print("Invalid color")
}
```

The code here is much easier to read and understand when compared to the previous version. The value in trafficLightColor is "Yellow", so case "Yellow": is matched and Caution is printed in the Debug area. Try changing the value of trafficLightColor to see different results.

There are two things to remember about switch statements:

- - switch statements in Swift do not fall through the bottom of each case and into the next one by default. In the example shown previously, once case "Yellow": is matched, case "Red":, case "Green":, and default: will not execute.
- - switch statements must cover all possible cases. In the example shown previously, any trafficLightColor value other than "Red", "Yellow", or "Green" will be matched to default: and Invalid color will be printed in the Debug area.

This concludes the section on if and switch statements. In the next section, you’ll learn about optionals, which allow you to create variables without initial values, and optional binding, which allows instructions to be executed if an optional has a value.

## Optionals and optional binding

Up until now, every time you have declared a variable or constant, you have assigned a value to it immediately. But what if you want to declare a variable first and assign a value later? In this case, you would use optionals.

7. Let’s assign a value to spouseName. Modify the code as shown:

```
var spouseName: String?
spouseName = "Nia"
print(spouseName ?? "No value in spouseName")
```

When your program runs, Nia appears in the Debug area.

8. Add one more line of code to join spouseName to another string as shown:

```
print(spouseName ?? "No value in spouseName")
let greeting = "Hello, " + spouseName
```

You’ll get an error, and the Debug area displays the error information and where the error occurred. This happens because you can’t join a regular string variable to an optional using the + operator. You will need to unwrap the optional first.

9. Click on the red circle to display possible solutions, and you’ll see the following:

![](img/0972ba626734a3986e095c788777818e_83_0.png)

Figure 3.4: Expanded error notification with the second fix highlighted

The second solution recommends force-unwrapping to resolve this issue. Force-unwrapping unwraps an optional whether it contains a value or not. It works fine if spouseName has a value, but if spouseName is nil, your code will crash.

10. Click Apply for the second solution, and you’ll see an exclamation mark appear after spouseName in the last line of code, which indicates the optional is force-unwrapped:

```
let greeting = "Hello, " + spouseName!
```

11. Run your program, and you’ll see Hello, Nia assigned to greeting, as shown in the Results area. This means that spouseName has been successfully force-unwrapped.

12. To see the effect of force-unwrapping a variable containing nil, set spouseName to nil:

```
spouseName = nil
```

Your code crashes and you can see what caused the crash in the Debug area:

```
No value in spouseName
__lldb_expr_15/ConditionalsAndOptionals.playground:47: Fatal error: Unexpectedly found nil while unwrapping an Optional value
```

Figure 3.5: Crashed program details in the Debug area

Since spouseName is now nil, the program crashed while attempting to force-unwrap spouseName.

A better way of handling this is to use optional binding. In optional binding, you attempt to assign the value in an optional to a temporary variable (you can name it whatever you like). If the assignment is successful, a block of code is executed.

13. To see the effect of optional binding, modify your code as follows:

```
spouseName = "Nia"
print(spouseName ?? "No value in spouseName")
if let spouseTempVar = spouseName {
    let greeting = "Hello, " + spouseTempVar
    print(greeting)
}
```

Hello, Nia will appear in the Debug area. Here’s how it works. If spouseName has a value, it will be unwrapped and assigned to a temporary constant, spouseTempVar, and the if statement will return true. The statements between the curly braces will be executed and the greeting constant will then be assigned the value Hello, Nia. Then, Hello, Nia will be printed in the Debug area. Note that the temporary variable spouseTempVar is not an optional. If spouseName does not have a value, no value can be assigned to spouseTempVar and the if statement will return false. In this case, the statements in the curly braces will not be executed at all.

14. You can also write the code in the previous step in a simpler way as follows:

```
spouseName = "Nia"
print(spouseName ?? "No value in spouseName")
if let spouseName {
    let greeting = "Hello, " + spouseName
```

Check out the following video to see the code in action:

https://youtu.be/U52X9c9EAVA

If you wish to start from scratch, create a new playground and name it RangeOperatorsAndLoops.

You can type in and run all the code in this chapter as you go along.

Let’s start by specifying a range of numbers using range operators.

## Range operators

Imagine you need to write a program for a department store that automatically sends a discount voucher to customers between the ages of 18 and 30. It would be very cumbersome if you needed to set up an if or switch statement for each age. It’s much more convenient to use a range operator in this case.

Range operators allow you to represent a range of values. Let’s say you want to represent a sequence of numbers starting with firstNumber and ending with lastNumber. You don’t need to specify every value; you can just specify the range in this way:

```
firstNumber...lastNumber
```

In the department store example, you would specify the customer ages as follows:

```
18...30
```

For more information on range operators, visit https://docs.swift.org/swift-book/documentation/the-swift-programming-language/basicoperators.

Let’s try this out in the playground. Follow these steps:

- 1. Add the following code to your playground and run it:

```
let myRange = 10...20
```

This will assign a number sequence that starts with 10 and ends with 20, including both numbers, to the myRange constant. This is known as a closed-range operator. The start and end values for myRange will be displayed in the Results area:
