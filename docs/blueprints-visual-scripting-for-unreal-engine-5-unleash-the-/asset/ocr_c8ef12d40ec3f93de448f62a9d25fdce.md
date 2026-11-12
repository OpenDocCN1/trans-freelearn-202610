+   3. Drag a wire from the white output pin of the SET Current Patrol Point node and add the UpdatePatrolPointBB macro node:

![](bbox=[0.125, 0.155, 0.865, 0.275])

Figure 9.33 – Setting an initial patrol point

+   4. Now, let's create the Event ActorBeginOverlap event, which swaps the patrol points. The event first checks whether the enemy overlaps with Patrol Point 1. If it is true, then the event sets Patrol Point 2 as Current Patrol Point. If it is false, then the event checks whether the enemy overlaps with Patrol Point 2. In this case, the event sets Patrol Point 1 as Current Patrol Point:

![](bbox=[0.125, 0.49, 0.87, 0.72])

Figure 9.34 – Swapping the patrol points

+   5. The nodes of steps 5–8 are to check whether the enemy overlaps with Patrol Point 1. Drag a wire from the white execution pin of the Event ActorBeginOverlap node and add a Branch node.

+   6. Drag a wire from the Condition input pin of the Branch node and add an Equal node.