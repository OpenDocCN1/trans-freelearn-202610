
This protocol is named CalorieCountable. It specifies that any object that adopts it must have a property, calories, that holds the calorie count, and a method, description(), that returns a string. { get } means that you only need to be able to read the value stored in calories, and you don’t have to write to it. Note that the definition of the description() method is not specified, as that will be done in the class, structure, or enumeration. All you need to do to adopt a protocol is type a colon after the class name, followed by the protocol name, and implement the required properties and methods.

To make the Burger class conform to this protocol, modify your code as follows:

```
class Burger: CalorieCountable {
    let calories = 800
    func description() -> String {
        "This burger has \(calories) calories"
    }
}
```

As you can see, the calories property and the description() method have been added to the Burger class. Even though the protocol specifies a variable, you can use a constant here because the protocol only requires that you get the value for calories, not set it.

Let’s make the Fries structure adopt this protocol as well. Modify your code for the Fries structure as follows:

```
struct Fries: CalorieCountable {
    let calories = 500
    func description() -> String {
        "These fries have \(calories) calories"
    }
}
```

The code added to the Fries structure is like that added to the Burger class, and it now conforms to the CalorieCountable protocol as well.

You could modify the Sauce enumeration in the same way, but let’s do it using extensions instead. Extensions extend the capabilities of an existing class, structure, or enumeration. You’ll add the CalorieCountable protocol to the Sauce enumeration using an extension in the next section.

2. To get the total calorie count, add the following code after the line where you created the foodArray constant:

```
let totalCalories = foodArray.reduce(0, {$0 + $1.calories})
print(totalCalories)
```

The reduce method is used to produce a single value from the elements of the foodArray array. The first parameter of this method is the initial value, and it is set to 0. The second parameter is a closure that combines the initial value with the value stored in an element’s calories property. This is repeated for each element in the foodArray array, and the result is assigned to totalCalories. The total amount, 1315, will be displayed in the Debug area.

You have learned how to create a protocol and make a class, structure, or enumeration conform to it, either within the class definition or via extensions. Let’s look at error handling next and see how to respond to or recover from errors in your program.

## Error handling

When you write apps, bear in mind that error conditions may happen, and error handling is how your app responds to and recovers from such conditions.

First, you create a type that conforms to Swift’s Error protocol, which lets this type be used for error handling. Enumerations are normally used, as you can specify associated values for different kinds of errors. When something unexpected happens, you can stop program execution by throwing an error. You use the throw statement for this and provide an instance of the type conforming to the Error protocol, with the appropriate value. This allows you to see what went wrong.

Of course, it would be better if you could respond to an error without stopping your program. For this, you can use a do-catch block, which looks like this:

```
do {
    try expression1
    statement1
} catch {
    statement2
}
```

Here, you attempt to execute code in the do block using the try keyword. If an error is thrown, the statements in the catch block are executed. You can have multiple catch blocks to handle different error types.

+   3. Type the following code after the checkWebsite(siteUp:) function definition and run your program:

```
let siteStatus = true
try checkWebsite(siteUp: siteStatus)
```

Since siteStatus is true, Site is up will appear in the Results area.

+   4. Change the value of siteStatus to false and run your program. Your program crashes and the following error message is displayed in the Debug area:

```
Playground execution terminated: An error was thrown and was not caught.
```

+   5. Of course, it is always better if you can handle errors without making your program crash. You can do this by using a do-catch block. Modify your code as shown and run it:

```
let siteStatus = false
do {
    print(try checkWebsite(siteUp: siteStatus))
} catch {
    print(error)
}
```

The do block tries to execute the checkWebsite(siteUp:) function and prints the status if successful. If there is an error, instead of crashing, the statements in the catch block are executed, and the siteDown error message appears in the Debug area.

You have learned how to handle errors in your app without making it crash. Great!

## Summary

In this chapter, you learned how to write protocols and how to make classes, structures, and enumerations conform to them. You also learned how to extend the capabilities of a class by using an extension. Finally, you learned how to handle errors using the do-catch block.

These may seem rather abstract and hard to understand now, but in Part 3 of this book, you will see how to use protocols to implement common functionalities in different parts of your program instead of writing the same program over and over. You will see how useful extensions are in organizing your code, which makes it easy to maintain. Finally, you’ll see how good error handling makes it easy to pinpoint the mistakes you made while coding your app.

## 9

## Swift Concurrency

Apple introduced **Swift concurrency**, which adds support for structured asynchronous and parallel programming to Swift 5.5, during WWDC21. It allows you to write concurrent code, which is more readable and easier to understand. During WWDC24, Apple introduced **Swift 6**, which makes concurrency programming easier by diagnosing **data races** at compile time. Swift concurrency documentation was further updated after WWDC25.

> Currently, it is not recommended to turn strict concurrency on for large existing projects, as it is likely to generate multiple errors and warnings. However, as this is Apple’s direction going forward, you will be turning it on for the project in this chapter and in *Part 3* of this book, so you may learn and gain experience with it.

In this chapter, you will learn the basic concepts of Swift concurrency. Next, you will examine an app without concurrency and explore its issues. After that, you will use async/await to implement concurrency in the app. Finally, you’ll make your app more efficient by using async-let.

By the end of this chapter, you’ll have learned the basics of how Swift concurrency works and how to update your own apps to use it.

The following topics will be covered:

- Swift concurrency
- Examine an app without concurrency
- Update the app using async/await
- Improve efficiency using async-let

## Technical requirements

We will use an example app, *BreakfastMaker*, to understand the concepts of Swift concurrency.

The completed Xcode project for this chapter is in the Chapter09 folder of the code bundle for this book, which can be downloaded here:

https://github.com/PacktPublishing/iOS-26-Programming-for-Beginners-10E

Check out the following video to see the code in action:

https://youtu.be/QzsaoTmCp5s

Let’s start by learning about Swift concurrency in the next section.

## Swift concurrency

In Swift 5.5, Apple added support for writing **asynchronous** and **parallel** code in a structured way.

Asynchronous code allows your app to suspend and resume code. Parallel code allows your app to run multiple pieces of code simultaneously. This allows your app to do things such as update the user interface while still performing operations such as downloading data from the internet.

You can read Apple’s Swift concurrency documentation at this link: https://docs.swift.org/swift-book/documentation/the-swift-programming-language/concurrency/.

During WWDC24, Apple released Swift 6. With the Swift 6 language mode, the compiler can now guarantee that concurrent programs are free of data races. This means that code from one part of your app can no longer access the same area of memory that is being modified by code from another part of your app. However, when you create a new Xcode project, it defaults to the Swift 5 language mode, and you must turn on the Swift 6 language mode to enable this feature. After WWDC25, Apple updated the documentation and migration guide for Swift concurrency.

To view Apple’s documentation on migrating your app to Swift 6, click this link: https://www.swift.org/migration/documentation/migrationguide/.

To view Apple’s WWDC25 video on the latest updates to Swift concurrency, click this link: https://developer.apple.com/videos/play/wwdc2025/268/.
