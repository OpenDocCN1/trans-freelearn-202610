![](bbox=[0.126, 0.091, 0.869, 0.555])

# Figure 6.3 – Movement input Events

The **Select** nodes test whether the player is using a **virtual reality head-mounted display (VR HMD)**. If a VR HMD is enabled, then the vectors used are from **FirstPerson Camera**; if it isn't, then the vectors used are from the Actor root Component.

Functionally, the other nodes are set up similarly to the **Stick input** and **Mouse input** groups. The axis value is taken from the forward and right movement axis inputs on a controller or keyboard. Again, these nodes represent backward and left movements as well, in the form of negative values for the **Axis Value** outputs. The significant difference in movement translation is that we require the direction that the Actor is going to be moved in, so that the degree of movement can be applied in the correct direction. The direction is pulled from the **Get Actor Vector** nodes (both