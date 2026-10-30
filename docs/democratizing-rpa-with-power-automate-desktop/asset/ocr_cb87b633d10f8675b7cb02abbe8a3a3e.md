Figure 6.6 – Subflow to extract new orders

These are some comments on the different steps (line numbers).

*Step 2* starts Microsoft Access with the Northwind Database. I used two variables here. `MSAccessApp` contains the full path to the executable of Microsoft Access, while `NorthWindDB` contains the path to the database itself.

In *step 3*, we use the first UI action, called **Wait for window**. Old programs or computers with less power (processor, RAM, and so on) might have some latency when starting a program. This action will pause the flow until the given window appears. In this case, the flow waits for the login screen to appear. The login window was captured as a UI element previously.

*Step 4* triggers the click on the login button. As shown in *Figure 6.2*, the database application first wants a user to log in. Here, we just take the first user and press the login button.

Now we should wait for the main application window to appear. That’s why we use the **Wait for window** action again in *step 5*.

In *step 6*, I used an image to be able to address the **view orders** link on the right-hand side of the application. The recorder was not able to detect this as a separate UI element, so I had to use the image functionality of PAD to move the mouse to this link and click on it. This is a good example showing that even those cases can be addressed by PAD.

In *steps 8 to 11*, I am using MS Access ribbon buttons to filter the list of orders so that it only contains new orders, selecting all of them, and copying them to the clipboard. This allows you to parse the content in the next subflow.

Please notice that we are not closing the application at this point.

# The ExtractNewOrders subflow