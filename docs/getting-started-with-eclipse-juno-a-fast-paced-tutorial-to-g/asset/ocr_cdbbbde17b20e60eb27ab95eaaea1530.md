```
public int getResult() {
    return result;
}
}
```

# Downloading the example code

You can download the example code files for all Packt books you have purchased from your account at http://www.PacktPub.com. If you purchased this book elsewhere, you can visit http://www.PacktPub.com/support and register to have the files e-mailed directly to you.

![](bbox=[0.22, 0.26, 0.26, 0.31])

Our calculator implementation contains two faults, but do not mind about them yet. Using a buggy implementation will make our example more interesting.

The purpose of the BasicCalculator class is to perform elementary arithmetic operations (addition, subtraction, multiplication, and division) and calculate the square root; while storing the result after each operation. Apart from the result variable, an array is used to store preliminary results; making it possible to reuse it in subsequent calculations. Methods for reusing the values stored in this array are not implemented yet. So far, our calculator can only store preliminary results (which are kept in the tempVars array) and clean its memory (by assigning zero to all elements of tempVars). Further, notice that the method that should calculate the square root is not implemented either.

![](bbox=[0.22, 0.57, 0.27, 0.62])

Well, our class should calculate the square root, however, as you saw, this particular feature has not been implemented yet. Do not worry about that, we did not implement this particular feature for demonstration purposes. Later in this chapter, we will show you how to make JUnit skip a test case for which either there is no implementation or the current implementation has been acting up.

Since we are sticking to the naming convention we presented in the previous section, before testing BasicCalculator, you should create a package called chapter3.basiccalc.test. The class containing all test cases should be created inside this package.

Now that you have set your Java project up and dealt with all scaffolding around your test code, you can either add JUnit to your project build path following the aforementioned directions, or keep reading and discover an even easier way of adding JUnit to the build path (as we mentioned, there are many ways of doing operations in Eclipse).