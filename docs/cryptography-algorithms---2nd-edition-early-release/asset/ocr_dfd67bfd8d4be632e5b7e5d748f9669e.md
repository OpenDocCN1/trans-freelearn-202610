![](bbox=[0.134, 0.129, 0.869, 0.469])

In this case, the result of the operation is **(10001011)₂ = 139** expressed in decimal notation.

+   - This operator represents the modular **sum (X+Y) (mod 2^32)** and is used in SHA to represent this operation, as you will see later in our examination of the SHA-1 algorithm (*Figure 4.14*).

After having analyzed the logic operators used to perform hash functions, two more issues have to be considered in order to implement hash functions:

+   - In SHA, for example, there are some constants (**Kt**) that go from **0 to n** (we will look at this in the following section).
+   - The notations are generally expressed in hexadecimal.

Let's now see how the hexadecimal system works. It uses the binary numbers for 0 to 9, consisting of 4 bits per number, for example: 0 = 0000, 1 = 0001, 2 = 0010, … 9 = 1001. Then, from 10 to 16 we have 6 capital letters, **A, B, C, D, E, F**, to reach a total of 16 (hex).