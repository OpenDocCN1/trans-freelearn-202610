![](bbox=[0.125, 0.09, 0.87, 0.52])

# Figure 6.1 – Stick input Events

The red Event nodes are triggered at every frame and pass the **Axis Values** of **TurnRate** and **LookUpRate** from a controller input. These values are usually mapped to the left/right and up/down axis triggers of an analog stick. Note that there are only two axis triggers. Detecting a look down or a turn left Event is covered by these very nodes and is represented as a negative number in the **Axis Value** that is passed.

Then, the values from the two axis triggers are each multiplied by a variable, representing the base rate at which the player is intended to be able to turn around or look up or down. The values are also multiplied by the world delta seconds to normalize against varying frame rates, despite the triggers being called every frame. The value resulting from multiplying all the three inputs is then passed to the **Add Controller Pitch Input** and **Add Controller Yaw Input** functions. These are the functions that add