8. Choose **Formulas | Show Formulas** to display the difference between the two columns. As shown in **Figure 7.14**, the formulas in the **Miles** column take the form of =D2/5280, while all cells in the **Kilometers** column contain the =[@Meters]/1000 formula, where =[@Meters] is a **structured reference** that refers to a **Header** instead of a cell reference:

![](bbox=[0.1504, 0.1938, 0.8711, 0.385])

Figure 7.14 – Show Formulas mode

Here are a couple of things to keep in mind about structured references:

+   • The **Miles** column does not use structured references because we typed in the cell address as opposed to selecting the cell. Conversely, in the **Kilometers** column, we typed an = sign and then used the left arrow key or mouse to select cell G2. Excel creates structured references automatically when you click on or navigate to a cell within a **Table**.

+   • To disable automatic structured references, choose **File | Options | Formulas**, clear the **Use Table Names in formulas** option, and then click on **OK**.

+   • Structured references should return the same data as cell references, but sometimes they might return the value from the first **Body Row** of a **Table** in every cell. In such situations, I resolve the problem by replacing the **Header** with a cell reference instead.

+   • In this context, the @ symbol is known as the **This Row** operator and instructs the formula to refer to the cell on the same row as the formula, as opposed to the entire **Table** column.

9. Choose **Formulas | Show Formulas** to return them back to showing results instead of formulas.

**Nuance**
Microsoft’s documentation states that the keyboard shortcut for **Show Formulas** is `Ctrl + ‘` (grave accent) in Windows or Mac. The tilde (~) character resides on the same key, so users will often say press `Ctrl + ~` because many Excel users would be hard-pressed to identify the grave accent on a keyboard.

Now, let’s see how to create self-resizing formulas that automatically include new data that is added to a **Table**.