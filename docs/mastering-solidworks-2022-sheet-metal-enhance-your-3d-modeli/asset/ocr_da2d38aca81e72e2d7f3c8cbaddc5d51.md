To understand what exactly the **Bend Allowance** property is and why it is important, let's look at a simple *part* from the side. This consists of a **Base Flange** and one single **Edge Flange**, as illustrated in Figure 1.11.

![](bbox=[0.25, 0.2, 0.75, 0.52])

Figure 1.11: Understanding the Bend Allowance property

We can see that the size of the Sheet Metal *part* will be the length of the **Base Flange** (A in Figure 1.11) plus the length of the **Edge Flange** (B in Figure 1.11), plus the size of the area of the bend itself (**BA** or Bend Allowance in Figure 1.11). Therefore, when the *part* is flattened, the total size of the sheet that we require will be **A + B + BA**.

However, depending on how and where the bend length (**BA**) is measured, we may end up with different values. For example, if the inside of the bend is measured, the length may be 5mm, but if the outside of the bend is measured then the length will be longer—15mm in this case, as illustrated in Figure 1.12.

![](bbox=[0.38, 0.75, 0.62, 0.88])

Figure 1.12: The size of the bend depends on where exactly is measured