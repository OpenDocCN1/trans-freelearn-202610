# Hinge

The **Hinge** constraint acts as a hinge for a door. It creates a rotation point that's based on the location of the **Empty** object. The rotation is also constricted to the local Z-axis for the **Empty** object. While you can rotate the **Empty** object at a different angle, make sure you have the Z letter pointing in the direction you want the hinge to be:

![](bbox=[0.34, 0.2, 0.66, 0.36])

Figure 10.32 – Hinge constraint

In the settings for the **Hinge** constraint, there's a new option: **Z Angle**. Turning this option on allows you to limit the rotation of the hinge using the **Upper** and **Lower** values. Without any limits, the hinge will be able to spin all the way around without stopping.

# Slider

The **Slider** constraint allows the object to slide along the local X-axis of the **Empty** object. The movement is restricted to the X-axis, so make sure you rotate the **Empty** object in the direction you want the slide to happen.

In the settings for the constraints, there's an **X-Axis** checkbox. With this enabled, you can set how far away the rigid body can slide using the **Upper** and **Lower** values:

![](bbox=[0.21, 0.61, 0.79, 0.8])

Figure 10.33 – Slider constraint