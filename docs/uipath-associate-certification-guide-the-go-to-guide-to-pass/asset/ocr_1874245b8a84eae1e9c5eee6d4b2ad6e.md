27. Run the workflow and check the output of EmployeeDT. You will see the final outcome of the manipulations we performed on the data table. Feel free to use the combination of Output Data Table and Write Line anywhere in the workflow to explore the output after each manipulation action.

28. Use a Join Data Tables activity to join the two data tables using the common Department ID. Configure the Join Data Tables activity, as shown in the following figure. Note that the Join Type option is set to an Inner join:

![](bbox=[0.115, 0.255, 0.85, 0.755])

# Figure 9.13 – Configuring the Join Data Tables activity

29. Use another combination of the Output Data Table and Write Line activities pointing to JoinDT (the data table that contains the result after joining) and examine the output.