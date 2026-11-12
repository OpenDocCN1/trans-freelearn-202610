![](bbox=[0.128, 0.091, 0.714, 0.489])

Figure 17.17 – Anim Preview Editor

Assign different values to the **Speed** variable between 0 and 375 and see the resulting animation in the Viewport.

We saw how to connect the animation nodes directly to the **Output Pose** node, but the **AnimGraph** was created with a State Machine in mind. We will discuss State Machines in the following section.

# Exploring State Machines

A State Machine in the **AnimGraph** allows you to organize the animation into a series of states. To exemplify this, we will create a State Machine with two states: idle and moving.

We need to define **Transition Rules** to control the transition from one state to another.