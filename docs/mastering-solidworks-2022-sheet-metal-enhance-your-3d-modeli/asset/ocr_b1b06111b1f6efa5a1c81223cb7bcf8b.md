7. Ensure that the **Open bend region** option is checked. This will automatically trim away a corner relief area when filling the gap. It is possible to create Closed Corners without this option selected (*Figure 9.5*) but it can result in manufacturing complications and can sometimes cause model errors if the **Gap distance** is too small.

![](bbox=[0.22, 0.22, 0.75, 0.5])

Figure 9.5: Closed Corner with Open bend region selected (left) and deselected (right)

8. Press the green OK check mark to create the **Closed Corner**. Note that although we do still have a small gap (due to the nature of sheet metal, with folded flanges) it is much reduced.

9. There are also three other options that can be used with the **Closed Corner** tool. These can be seen by editing the **Closed Corner** feature:

+   - **Coplanar faces**: Selecting this option will mean that **Closed Corner** will be applied to all faces that are *coplanar* (lined up) with the selected faces, even if those coplanar faces are not selected themselves. This can be useful if one of the flanges has a cut in it that breaks the surface up into smaller sections (*Figure 9.6*):