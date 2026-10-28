
## Use custom argument labels

Note that the serviceCharge(mealCost:) function is not very English-like. You can add a custom label to the parameter to make the function easier to understand.

Modify your code as shown:

```
func serviceCharge(forMealPrice mealCost: Int) -> Int {
    mealCost / 10
}
let serviceChargeAmount = serviceCharge(forMealPrice: 50)
print(serviceChargeAmount)
```

The function works the same as before, but to call it, you use serviceCharge(forMealPrice:). This sounds more like English and makes it easier to figure out what the function does. Also, note that if your function body only consists of a single statement, the return keyword is optional.

In the next section, you'll learn how to use several smaller functions within the bodies of other functions, and these are known as nested functions.

## Use nested functions

It's possible to have a function within the body of another function, and these are called nested functions. This allows you to keep several related functions together in one place and makes the enclosing function easier to understand.

A nested function can use the variables of the enclosing function. Let's see how nested functions work by writing a function to calculate monthly payments for a loan.

Type in and run the following code:

```
func calculateMonthlyPayments(carPrice: Double, downPayment: Double, interestRate: Double, paymentTerm: Double) -> Double {
    func loanAmount() -> Double {
        carPrice - downPayment
    }
    func totalInterest() -> Double {
        interestRate * paymentTerm
    }
    func numberOfMonths() -> Double {
        paymentTerm * 12
    }
```

```
return ((loanAmount() + (loanAmount() *
    totalInterest() / 100 )) / numberOfMonths())
}
calculateMonthlyPayments(carPrice: 50000, downPayment: 5000, interestRate: 3.5, paymentTerm: 7.0)
```

Here, there are three functions within calculateMonthlyPayments(carPrice:downPayment:interestRate:paymentTerm:). Let’s look at them:

- The first nested function, loanAmount(), calculates the total loan amount by subtracting downPayment from carPrice. It returns 50000 - 5000 = 45000.
- The second nested function, totalInterest(), calculates the total interest amount incurred for the payment term by multiplying interestRate by paymentTerm. It returns 3.5 * 7 = 24.5.
- The third nested function, numberOfMonths(), calculates the total number of months in the payment term by multiplying paymentTerm by 12. It returns 7 * 12 = 84.

Note that the three nested functions all use the variables of the enclosing function. The value returned is ( 45000 + ( 45000 * 24.5 / 100 ) ) / 84 = 666.96, which is the amount you must pay monthly for seven years to buy this car.

As you have seen, functions in Swift are like functions in other languages, but they have a cool feature. Functions are first-class types in Swift, so they can be used as parameters and return types. Let’s see how that is done in the next section.

## Use functions as return types

A function can return another function as its return type. Type in and run the following code to create a function that generates the value of pi using one of two possible ways:

```
func approximateValueOfPi1() -> Double {
    3.14159
}
func approximateValueOfPi2() -> Double {
    22.0 / 7.0
}
```

```
func pi() -> (() -> Double) {
    approximateValueOfPi1
    // approximateValueOfPi2
}
pi()()
```

Both approximateValueOfPi1() and approximateValueOfPi2() are functions that have no parameters and return the approximate value of pi. The pi() function’s return type is a function that has no parameters and returns Double. This means that it can either return approximateValueOfPi1 (as shown here) or approximateValueOfPi2, since both functions match the expected return type.

pi()() calls the approximateValueOfPi1 function, which returns 3.14159. 3.14159 is displayed in the Results area.

Let’s see how a function can be used as a parameter for another function in the next section.

## Use functions as parameters

A function can take another function as a parameter. Type in and run the following code to create a function that determines whether a number meeting a certain condition exists within a list of numbers:

```
func isThereAMatch(listOfNumbers: [Int], condition: (Int) -> Bool) -> Bool
{
    for number in listOfNumbers {
        if condition(number) {
            return true
        }
    }
    return false
}
func numberIsOdd(number: Int) -> Bool {
    (number % 2) > 0
}
func numberIsEven(number: Int) -> Bool {
    (number % 2) == 0
}
let numbersList = [1, 3, 5, 7]
isThereAMatch(listOfNumbers: numbersList, condition: numberIsOdd)
```

isThereAMatch(listOfNumbers:condition:) has two parameters: an array of integers and a function. The function provided as an argument must take an integer value and return a Boolean value.

Both numberIsOdd(number:) and numberIsEven(_:) take an integer and return a Boolean value, which means either function can be an argument for the second parameter. numbersList, an array containing odd numbers, is used as the argument for the first parameter. When numberIsOdd is used as an argument for the second parameter, isThereAMatch(listOfNumbers:condition:) will return true when called. Try using numberIsEven as an argument for the second parameter as well.

Functions as parameters and return types can be difficult to understand but are relatively rare at this point in your learning journey, so don’t worry if you don’t get it at first. As you gain experience, it will become clearer to you.

In the next section, you’ll see how you could perform an early exit on a function if the arguments used are not suitable.

## Use a guard statement to exit a function early

Let’s say you need a function to be used in an online purchasing terminal. This function will calculate the remaining balance of a debit or credit card when you buy something. The price of the item that you want to buy is entered in a text field.

The value in the text field is converted into an integer so that you can calculate the remaining card balance. If there is something wrong with the input data, it is useful to be able to exit a function early.

Type in and run the following code:

```
func buySomething(itemValueEntered itemValueField: String, cardBalance: Int) -> Int {
    guard let itemValue = Int(itemValueField) else {
        print("Error in item value")
        return cardBalance
    }
    let remainingBalance = cardBalance - itemValue
    return remainingBalance
}
print(buySomething(itemValueEntered: "10", cardBalance: 50))
print(buySomething(itemValueEntered: "blue", cardBalance: 50))
```

You should see this result in the Debug area:

```
40
Error in item value
50
```

Let’s see how this function works. The first line in the function body is a guard statement. This checks to see whether a condition is true; if not, it exits the function. Here, it is used to check and see whether the user entered a valid price in the online purchasing terminal.

If so, the value can be converted successfully into an integer, and you can calculate the remaining card balance. Otherwise, the else clause in the guard statement is executed. An error message is printed to the Debug area and the unchanged card balance is returned.

For print(buySomething(itemValueEntered: "10", cardBalance: 50)), the item price is deducted successfully from the card balance, and 40 is returned.

For print(buySomething(itemValueEntered: "blue", cardBalance: 50)), the guard statement’s condition fails, and its else clause is executed, resulting in an error message being printed to the Debug area and 50 being returned.

You now know how to create and use functions. You have also seen how to use custom argument labels, nested functions, functions as parameters or return types, and the guard statement.

Now, let’s look at closures. Like functions, closures allow you to combine several instructions, but closures do not have names and can be assigned to a constant or a variable. You’ll see how they work in the next section.

## Closures

A closure, like a function, contains a sequence of instructions and can take arguments and return values. However, closures don’t have names. The sequence of instructions in a closure is surrounded by curly braces ({ }), and the in keyword separates the arguments and return type from the closure body.

Closures can be assigned to a constant or variable, so they’re handy if you need to pass them around inside your program. For instance, let’s say you have an app that downloads a file from the internet, and you need to do something to the file once it has finished downloading. You can put a list of instructions to process the file inside a closure and have your program execute it once the file finishes downloading.
