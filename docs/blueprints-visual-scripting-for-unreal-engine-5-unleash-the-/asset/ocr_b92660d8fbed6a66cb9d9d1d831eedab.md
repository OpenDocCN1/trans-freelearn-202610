![](bbox=[0.123, 0.091, 0.866, 0.443])

# Figure 18.19 – Creating Actor and Scene Components

The Scene Component is a Child class of the Actor Component that has the Transform structure (location, rotation, and scale). Because of the Transform, a Scene Component can be attached to another Scene Component. We will explore the Scene Component in the next section.

When scripting a component in the EventGraph, you can get a reference of the Actor that is using the component with the **Get Owner** node:

![](bbox=[0.123, 0.674, 0.55, 0.82])

# Figure 18.20 – Getting the reference of the Actor that is using the component