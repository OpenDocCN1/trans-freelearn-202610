The K-Factor is a ratio, so it will always be between 0 and 1. The inside of the bend is a K-Factor of 0 and the outside of the bend is a K-Factor of 1. Therefore, the middle of the bend is a K-Factor of 0.5, as illustrated in Figure 1.14.

![](bbox=[0.32, 0.19, 0.72, 0.31])

Figure 1.14: The K-Factor is a ratio indicating a line through a cross-section of the bend

**What Value of K-Factor Should I Use?**
The K-Factor depends on a number of factors, such as material and bending type. If you're unsure about what to use, then a K-Factor of 0.5 is usually a good estimate for most parts and materials.
You can also try asking your manufacturer what value they recommend you should use.

The K-Factor is the easiest way to work out **Bend Allowances** and can easily be changed at a later stage if needed, by editing the **Sheet Metal** properties.

# Other Bend Allowance options

As well as the K-Factor, there are also some other ways of calculating the **Bend Allowance**. If you are unfamiliar with these methods, then it is highly recommended that you use the **K-Factor** option, as detailed previously. The other **Bend Allowance** options are:

+   - **Bend Allowance**: This allows you to specifically define the length of the **Bend Allowance**. This can be useful if a manufacturer gives you a certain value to use. However, this option should be used with care as any value can be inputted. Therefore, if an incorrect value is used, this can result in completely wrong sizes on **Flat Pattern** parts.
+   - **Bend Deduction**: This option is somewhat similar to the **Bend Allowance** option but it measures the outside length of the folded *part*, then removes (or deducts) a certain value. Again, if you are unfamiliar with this method, use it cautiously, as using the wrong **Bend Deduction** value can give unusable results.