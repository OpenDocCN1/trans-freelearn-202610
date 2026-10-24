

# Here's how to unblock the file:

- 1. Close the Chapter 11 - BOX_VOLUME and XBOX_VOLUME.xlsm workbook.
- 2. Use File Explorer to navigate to the folder where you saved the workbook.
- 3. Right-click on the file and choose Properties.
- 4. As shown in Figure 11.29, click Unblock and then click OK:

![](img/0c7a357df7e57e06b6d9f631c219040c_379_0.png)

# Figure 11.29 – The Microsoft Windows File Properties dialog box

- 5. Open the workbook and then click Enable Content when prompted if you want to be able to try the custom worksheet functions.

# Here's how to test the functions:

- 1. Enter =BOX_VOLUME(C3,C4,C5) in cell G3 of the User-Defined Functions worksheet. The formula should return 3,456.

> Nuance
> Custom worksheet functions written in VBA do not offer the ScreenTip functionality offered by LAMBDA, so you’ll have to remember which arguments to enter, and in what order.

- 2. Enter =XBOX_VOLUME(C8:D10) in cell G8 of the User-Defined Functions worksheet.
The formula should return 3,456.

You should find that both functions work in the same fashion as the LAMBDA functions. Both functions have an underscore in their name to distinguish them from the LAMBDA functions.

Now, let’s learn how to create the BOX_VOLUME function in VBA, which, unlike the LAMBDA function, will work in any version of Excel for Windows or Excel for macOS. The function name includes an underscore to distinguish it from the LAMBDA-based function we created in the previous section. Here are the steps:

- 1. Erase cells G3 and G8.
- 2. Choose Home | Format | Move or Copy Sheet (or right-click on the Worksheet tab and then choose Move or Copy).
- 3. Choose (new book) from the To book list.
- 4. Click the Create a copy checkbox so that you don’t inadvertently move the worksheet.
- 5. Click OK to move a copy of the worksheet to a new workbook.
- 6. Press Alt + F11 (Option + F11) to display Excel’s Visual Basic Editor or choose Developer | Visual Basic if you have enabled the Developer tab in Excel’s ribbon.
- 7. Choose Insert | Module within Visual Basic Editor.
- 8. Enter the following programming code into the module sheet:

```
Function BOX_VOLUME(length, height, width)
BOX_VOLUME = length * height * width
End Function
```

- 9. Activate the User-Defined Functions worksheet in your new workbook.
- 10. Enter =BOX_VOLUME(C3,C4,C5) in cell G3.

If cells C3, C4, and C5 contain valid amounts, and you specify all three arguments, the BOX_VOLUME function will compute the amounts; otherwise, it will return #VALUE!. In this case, length, height, and width are Variables that are used to store data used by the function. The words Function and End Function inform Excel that this segment of code is a calculation as opposed to a subroutine that would carry out a task. Subroutines can carry out calculations as well, but only when the macro is triggered, whereas functions recalculate automatically.

Granted, this is a simplistic worksheet function that is on par with using LET. VBA opens a whole world of possibilities, such as a worksheet function that you can pass a range of cells to, as we did with LAMBDA.

# Nuance

Workbooks that contain user-defined functions must be saved as **macro-enabled workbooks** (.XLSM files); otherwise, the programming code that you added for the custom function will be discarded. This also means that you’ll need to enable macros each time you open the workbook. I discussed how to suppress this prompt in Chapter 2, Disaster Recovery and File-Related Prompts.

The Chapter 11 - BOX_VOLUME and XBOX_VOLUME.xlsm workbook also contains the programming code for the XBOX_VOLUME function. The following steps will enable you to see the programming code for that function. You can use these steps whenever you want to edit a function that you have created:

- 1. Press Alt + F11 (Option + F11) to display Excel’s Visual Basic Editor or choose Developer | Visual Basic.
- 2. Choose View | Project Explorer or press Ctrl + R (Ctrl + ⌘ + R).
- 3. Expand the corresponding VBAProject for Chapter 11 - BOX_VOLUME and XBOX_VOLUME.xlsm, and then expand the Modules folder within.
- 4. Double-click on the modBOX_VOLUME module sheet to display the programming code. You can edit the function if you wish.

# Tip

To remove a custom worksheet function written in VBA, select the programming code starting from the word Function down through End Function and then press Delete. Another approach is to choose File | Save As and save the workbook as an Excel Workbook, as opposed to a Macro-Enabled Workbook. Keep in mind that doing so will remove all Visual Basic programming code from the workbook, not just custom functions.

In this chapter, I was only able to give you a small nudge in the direction of using LAMBDA and VBA functions but hopefully, you can see the immense power that custom functions can bring. LAMBDA functions have the potential to make complex calculations in Excel far more accessible for countless spreadsheet users without them requiring any knowledge of programming code. Furthermore, LAMBDA functions can empower more users to harness Excel’s power because performing complex calculations can become as simple as passing a few Parameters to a pre-written LAMBDA function.

# Summary

This chapter led you down a path that started with computing the cubic volume of a box with simple multiplication. Then, we layered on more and more complexity as we made our way to the LET and LAMBDA functions. The phrase game-changing often feels trite, but if you write complex formulas in Excel, both LET and LAMBDA empower you to write formulas today that you won’t cringe at when you see them again in the future. I’ve thought “what planet was I on that day?” more than a few times throughout my career when revisiting formulas that span multiple rows in the Formula Bar area.

# Names, LET, and LAMBDA

We made several stops, including visiting functions such as `PRODUCT`, `IF`, `CHOOSE`, and `SWITCH`. I included these because of similarities in how you can pass information to the functions, or swap information around as stepping-stones toward `LET` and `LAMBDA`. From there, we embarked on creating **Names**, partly to correlate the similarities between the `LET` and `LAMBDA` functions, but also because **Names** are how we formalize `LAMBDA` into custom worksheet functions. Apart from that, **Names** create built-in navigation aids and streamline formula writing and auditing.

One of the biggest benefits of `LET` and `LAMBDA` is the ability to eliminate repetitive segments in formulas. I feel like `LET` offers the easiest approach, which is to store the repetitive calculation in a **Variable** that you then reference in the _calculation_ argument as needed. `LAMBDA` functions do require you to spell out repetitive calculations each time you need them in the calculation, but you only need to think about it when creating the `LAMBDA` function. Once you formalize the `LAMBDA` function with a **Name**, the repetitive calculations occur in the background without being seen, so you can simply focus on passing the correct inputs through to the custom worksheet function. I didn’t have enough pages to explore it, but you can use `LET` within a `LAMBDA` function and then have the best of both worlds.

Now that we have `LAMBDA` at our disposal, the final section in this chapter feels more like a “_not that would, but you could_” kind of thing. If you don’t have any programming experience, creating custom worksheet functions with VBA can feel like walking into the water at a beach and suddenly finding yourself in deep water. I included that section partly as an homage to Excel’s past, but primarily to draw a sharp contrast with how empowering `LAMBDA` feels for modern Excel users.

In the next chapter, we will explore **Power Query**, which enables you to connect your Excel workbooks to a variety of data sources, including Excel workbooks, text files, PDF files, databases, and more.

