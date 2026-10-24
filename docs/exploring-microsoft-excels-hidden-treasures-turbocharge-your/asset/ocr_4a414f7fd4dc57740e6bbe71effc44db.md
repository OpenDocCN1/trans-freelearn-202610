Parameters, although generally, you will want to have at least one. As you’ll see later in this chapter,
you can create optional Parameters that the user can opt to omit when using the LAMBDA function.

There are four stages to creating a worksheet function with LAMBDA:

+   - Use the LAMBDA function to name the Parameters and define the calculation
+   - Evaluate the LAMBDA function by making a function call that provides the specified inputs
+   - Formalize the LAMBDA function by assigning a Name
+   - Use the LAMBDA function in the same fashion as you would any built-in worksheet function

Let’s work through the first stage.

# # Naming Parameters and defining the calculation

Cell F3 of the LAMBDA Function worksheet, as shown in Figure 11.16, contains the
=LAMBDA(length,width,height,length*height*width) formula and returns #CALC!
because no inputs have been provided, meaning I didn’t add a function call yet:

![](bbox=[0.1318, 0.443, 0.8535, 0.5859])

Figure 11.16 – The LAMBDA function

The LAMBDA function can have as little as one argument, but will generally have two or more:

+   - parameter: This optional argument contains information that you want to pass to the
+     function, such as a cell reference, number, or text. The formula in cell F3 has three Parameters:
+     length, width, and height. You can enter up to 253 Parameters.
+   - calculation: This required argument must always be the last one in the formula and represents
+     a formula that you want to calculate. As with LET, you may think of this as a formula within
+     a formula. If you don’t provide a Parameter, then calculation will be the first argument.

**Tip**
You can designate Parameters as optional by enclosing the Name in square brackets, such as
[parameter]. Then, you can use the ISOMITTED function in the calculation argument
to determine whether the Parameter was provided or not.