![](bbox=[0.125, 0.085, 0.87, 0.31])

Figure 13.56 – An example of the Do Once node

When the player presses **Space Bar**, the **Do Once** node is triggered, and a charged weapon is used. After that, a timer is created to execute **Full charge Custom Event**, which resets the **Do Once** node after `30.0` seconds. If the player presses **Space Bar** again before 30 seconds have elapsed, then the **Do Once** node will not execute its output pin.

# Do N

The **Do N** node allows you to specify how many times the output pin can execute. After the number of executions has completed, the Actions of the output pin will only be executed again if the **Reset** pin is triggered.

The following screenshot shows an example of how the **Do N** node is used: