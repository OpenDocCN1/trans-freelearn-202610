# # Creating Scene Components

We will create a Scene Component named BP_CircularMovComp that rotates around the Actor. Then, we will attach a Static Mesh Component to the BP_CircularMovComp component to simulate a rotating shield.

Follow these steps to create our Scene Component:

+   1. Access the Chapter18 folder that we created in the first example of this chapter.

+   2. Click the Add button in the content browser and choose the Blueprint Class option.

+   3. On the next screen, choose Scene Component as the parent class. Name the Blueprint BP_CircularMovComp and double-click it to open the Blueprint Editor.

+   4. In the My Blueprint panel, create the RotationPerSecond and DeltaAngle variables of the Float type:

![](bbox=[0.126, 0.523, 0.533, 0.621])

Figure 18.39 – Creating the variables of the component

+   5. Compile the Blueprint and select the RotationPerSecond variable. In the Details panel, set DEFAULT VALUE to 180.0. This value is in degrees, so the component will complete a rotation around the Actor in 2 seconds:

![](bbox=[0.126, 0.791, 0.497, 0.881])