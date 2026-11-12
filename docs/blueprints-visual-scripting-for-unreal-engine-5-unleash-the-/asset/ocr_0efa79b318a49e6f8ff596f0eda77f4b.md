![](bbox=[0.125, 0.09, 0.865, 0.245])

Figure 14.4 – Getting and setting the actor rotation

The Scale variable of the Transform structure has a set of values for X, Y, and Z, which determines the scale on each axis. The following screenshot shows the nodes that are used to get and set an actor's scale:

![](bbox=[0.125, 0.395, 0.865, 0.58])

Figure 14.5 – Getting and setting the actor scale

When a Blueprint has Actor components, the transforms of those components are known as relative transforms because they are relative to the component's parent. The following screenshot shows an example of components. DefaultSceneRoot is a small white sphere that is hidden in the game and used to store the actor's position in the world. It can be replaced with another scene component.

Below it, in the component's hierarchy shown in the screenshot, there is a Static Mesh component named Table, and below the Table component in the hierarchy, there is another Static Mesh component named Statue. The transform of Table is relative to the DefaultSceneRoot transform, and the transform of the Statue component is relative to the Table transform. So, if