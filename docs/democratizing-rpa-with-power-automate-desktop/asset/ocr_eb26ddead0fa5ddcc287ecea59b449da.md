Since this action displays a message and provides buttons to dismiss the dialog, a variable called **ButtonPressed** is created. Please notice that I also changed the value for **Message box buttons** to **OK – Cancel**, which will not just give us one **OK** button in the dialog, but two. The value for the `ButtonPressed` variable will contain the name of the button that the user will press: **OK** or **Cancel**. With this, a flow designer could evaluate that value and perform actions, depending on the button that was pressed. The examples provided by Microsoft make heavy use of this functionality. As a reminder, you can access these examples in the main PAD window and click on **Examples** to the right of **My flows**:

![](bbox=[0.126, 0.327, 0.866, 0.691])

Figure 5.4 – Examples for PAD provided by Microsoft

Also, notice that we could disable the production of a variable with the corresponding toggle switch. This applies to all actions of PAD that could expose variables. If we are not interested in the content of the variable and won’t need this in further processing, this would also make sense and keep the list of variables clearer (see *Figure 5.3*).