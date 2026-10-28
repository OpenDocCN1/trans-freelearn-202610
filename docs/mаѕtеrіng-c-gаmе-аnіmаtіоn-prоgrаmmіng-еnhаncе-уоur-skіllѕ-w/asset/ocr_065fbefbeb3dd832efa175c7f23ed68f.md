+   - **Wait Node**
+   
+   The wait node was already shown as an example in Figure 9.2. The node will delay the execution between two other nodes by a configurable time.
+   
+   - **RandomWait Node**
+   
+   This is like the wait node, but an upper and lower limit can be set to the delay, enabling a bit more random behavior.
+   
+   - **Selector Node**
+   
+   The selector node also has a fixed delay, and after the delay time is up, a random output pin is activated.
+   
+   - **Sequence Node**
+   
+   A sequence node activates the output pins one after the other, starting with pin number one. As an extra feature, the sequence node will wait for the child node on the active output to finish. Waiting for a child is currently only implemented for the two wait node types; more types or a cascade with grandchild nodes can be added. See the section Practical sessions for ideas.
+   
+   - **Action Node**
+   
+   We added actions in Chapter 7. An action node allows us to trigger actions just like we did in Chapter 7 while controlling the instance with a keyboard and mouse, enabling the instances to not just walk around but jump, roll, punch, or wave, based on the state of the node tree.
+   
+   - **Event Node**
+   
+   An event node is triggered by an external event sent to the instance, for instance, when a collision happens. The event node also has a cooldown timer to ignore the same event for a small amount of time, avoiding erratic behavior by doing the same action on every update of the tree again if the event keeps being sent to the instance.
+   
+   - **Instance Movement Node**
+   
+   The instance node allows us to control movement state and movement direction, instance speed, and instance rotation within the node tree. Using an instance node is required to change between the main movement states, like idle, walking, and running. In addition, both speed and rotation can be randomized within the upper and lower limits to enable a more nondeterministic behavior.