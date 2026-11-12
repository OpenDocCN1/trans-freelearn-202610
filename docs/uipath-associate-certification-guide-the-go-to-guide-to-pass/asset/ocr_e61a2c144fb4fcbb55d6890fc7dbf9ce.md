+   5. In the **Default** column of **Variables**, define the default list as {1, 2, 3, 4, 5, 6, 7, 8, 9, 10}, as shown in the following screenshot:

| Name | Variable type | Scope | Default |
|---|---|---|---|
| Counters | Int32[] | Sequence | {1,2,3,4,5,6,7,8,9,10} |

Figure 6.47 – Setting the default values

+   6. Drag and drop the **For Each** activity from the **Activities** panel into the **Designer** panel, as shown in the following screenshot:

![](bbox=[0.29, 0.295, 0.71, 0.535])

Figure 6.48 – Creating a For Each activity

+   7. Update the default value inside the **For Each** field to Counter and the **in** field to **Counters**, as shown in _Figure 6.49_. In the **Body** branch, drag a **Write Line** activity and write "The Current item in process is " + Counter.ToString. As the data type of each item of **Numbers** is Int32, we need to convert them into String format for the item to be displayed in the **WriteLine** activity.

The project's outline sequence should look as follows: