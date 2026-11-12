state to `Idle` and drag a wire from the **Entry** to **Idle** state node:

![](bbox=[0.125, 0.13, 0.542, 0.283])

Figure 17.20 – Adding states

+   5. Add another state and rename it to `Moving`. Drag a wire from the outer border of the **Idle** state node and connect it to the outer border of the **Moving** state node. Drag another wire from the **Moving** state node to the **Idle** state node. The arrow indicates that a transition is possible between the states:

![](bbox=[0.125, 0.478, 0.822, 0.602])

Figure 17.21 – The states and their transitions

+   6. Double-click on the **Idle** state node to edit the state. Drag the **ThirdPersonIdle** Animation Sequence from the **Asset Browser** and drop it in the **AnimGraph**. Connect the white icons of the **Play ThirdPersonIdle** and **Output Animation Pose** nodes: