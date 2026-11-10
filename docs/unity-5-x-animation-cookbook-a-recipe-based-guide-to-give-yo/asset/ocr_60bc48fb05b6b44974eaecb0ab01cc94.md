+   - **Transition Offset**: This offsets the target animation in time. The value is in percentage of the animation. Setting it to 0.3 means that the target animation will start from 30 percent of its length instead of the beginning.
+   - **Interruption Source**: This setting tells Unity whether the transition can be interrupted by other transitions. You can set it to:
+     - **None**: the transition cannot be interrupted by any other transition.
+     - **Current State**: The transition can be interrupted by transitions from the state we are trying to transition from.
+     - **Next State**: The transition can be interrupted by transition from the state we are trying to transition to.
+     - **Current State then Next State**: The transition can be interrupted by the transitions of the state we are trying to transition from or by the transitions of the state we are trying to transition to. The transitions from the state we are trying to transition from take the priority.
+     - **Next State then Current State**: The transition can be interrupted by the transitions of the state we are trying to transition from or by the transitions of the state we are trying to transition to. The transitions from the state we are trying to transition to take the priority.
+     - **Ordered Interruption**: If set to false, this lets the transition be interrupted by other transitions independently of their order. If set to true, the order of the transitions matters.

# There's more...

When you create an Animator Controller, you can see three more nodes apart from your animation states:

+   - **Entry and Exit**: This node is used when you transition between state machines (Animator Controllers or substate machines). We will discuss it in detail in the Using Sub-State Machines in Animator Controller recipe in Chapter 6, Handling Combat.
+   - **Any state**: This node can be used as a helper to make a transition from any state to a given state. It is used when you have an animation that can be played anytime, for instance, a hit animation. We will discuss it in detail in the Using transitions from any state to play hit reactions recipe in Chapter 6, Handling Combat.