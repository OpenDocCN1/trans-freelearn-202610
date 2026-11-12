11. Let's create the second function of our Blueprint Function Library. In the **My Blueprint** panel, click the + icon at the right of **FUNCTIONS**:

![](bbox=[0.125, 0.175, 0.56, 0.375])

Figure 18.7 – Creating another function

12. Name the function `RollTwoDice`. Create an input parameter named `NumberOfFaces` of the **Integer** type. Click the > symbol to expand the options and set **Default Value** to 6.

13. Create output parameters named `Sum`, `Die1`, and `Die2`, all of the **Integer** type:

![](bbox=[0.125, 0.575, 0.65, 0.725])

Figure 18.8 – Creating the output parameters

14. In the EventGraph, we will do the same validation of the **RollOneDie** function to check whether **Number Of Faces** is greater than 1. Add the nodes of Figure 18.5. You can also access the **RollOneDie** function, and select and copy the validation nodes to paste into the **RollTwoDice** function.