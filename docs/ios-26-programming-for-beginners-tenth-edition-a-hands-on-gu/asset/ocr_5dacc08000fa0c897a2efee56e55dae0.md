# Use custom argument labels

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

# Use nested functions

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
}
```