We return 1 if the input is 0; otherwise, we calculate the **Factorial** recursively.

Did you notice the `error` part of the code? We can use the `error` keyword to raise a custom error message. So in the preceding code, we check if the input value is invalid, then we raise a custom error message. The following image shows the result of invoking a **Factorial** function with an invalid number:

![](bbox=[0.175, 0.28, 0.825, 0.44])

Figure 3.49: Raising an error when invoking a Factorial function with an invalid value

There are cases when we do not want to raise any error messages even if the input is not valid. Instead, we want to show a constant value. The following code shows another version of the `Factorial` function checking if the input value is a positive value, then it calculates the factorial, otherwise, it returns -1:

```
let
    Factorial =
        (ValidNumber as number) as number =>
            if Number.Sign(ValidNumber) = 1
            then
                if ValidNumber = 0
                then 1
                else ValidNumber * @Factorial(ValidNumber - 1)
            else -1
in
    Factorial
```