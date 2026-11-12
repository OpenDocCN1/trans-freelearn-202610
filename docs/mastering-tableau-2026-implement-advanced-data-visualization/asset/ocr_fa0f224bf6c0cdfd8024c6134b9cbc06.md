Using a LOD calculation to display these values is straightforward. Let's explore. Follow along with the exercise steps:

+   1. Select the Practical Include worksheet
+   2. Select the Superstore dataset

+   3. Create a calculated field named Per State INCLUDE with the following code:

```
{INCLUDE [State]:SUM([Sales])}
```

+   4. Drag Region to the Columns shelf, SUM(Sales) to the Rows shelf, and AVG(Per State INCLUDE) to the Details shelf. Be sure to change Per State INCLUDE to an average aggregation.

+   5. Add two Reference Lines by right-clicking on the Sales axis and selecting Add Reference Line. Use the following settings:

![](bbox=[0.09, 0.338, 0.862, 0.701])

# Figure 7.41: Reference lines

+   6. Complete the worksheet by formatting it as desired: