![](bbox=[0.147, 0.092, 0.855, 0.472])

Figure 3.11: ArithValue.sol compiled with warning owing to not mentioning pragma directive

# Let us get back to our code:

+   1. First, we start by defining the contract. We name it `ArithValue`, as we are going to store and manipulate a value using arithmetic operations. There are no hard-and-fast naming rules here. Try to stick to some logical names.
+   
+   2. Then, we declare an unsigned integer value that will store our default number, which must be a positive integer.
+   
+   3. Now we need to give our contract a constructor function. So, what is a constructor function? Basically, this is a function that will be called whenever our contract is deployed and initiated for the first time. What's special about this constructor function is that it has the same name as that of the contract, that is, `ArithValue`. Inside the constructor function, we assign `100` to the number variable. So, now,