Now that our filter and parameter are connected, the filter will inherit the default values
of our parameter. In our example, we have used a multi-value parameter. We can confirm
that the pivot table only displays the records that contain the default values of the
parameter. Now that we have linked the parameter with the visual, we will learn how to
add on-screen controls and allow readers to change the parameter value.

# Adding on-screen controls

QuickSight allows you to add on-screen controls so that your reader users can set the
value of a parameter dynamically. In our example, setting the `paymenttype` parameter
value will result in changes in the value of the **Payment Type** filter, which has been
applied to the pivot table visual. To add an onscreen control, follow these steps:

+   1. First, select the **Parameters** item from the left-hand side menu.
+   2. Then, select the `paymenttype` parameter we created earlier. From the drop-down
+   menu, click **Add control**, as shown here:

![](bbox=[0.3457, 0.4219, 0.6543, 0.5742])

Figure 5.7 – Adding on-screen controls

+   3. From the control’s creation settings, we will need to define the following parameters:
+   
+   • **Display Name**: A user-friendly name for the on-screen control. In our example,
+   we used `Payment Type`.
+   
+   • **Style**: The type of control that your users will be using to set the parameter. The
+   control styles options are also determined by the data type of the parameter. For
+   example, if our parameter was a date type, **Date Picker** would be among the
+   available options. For our example, we will use the **List – Multiselect** style, as
+   shown in the following screenshot. This type will allow our users to select multiple
+   values from a drop-down menu.