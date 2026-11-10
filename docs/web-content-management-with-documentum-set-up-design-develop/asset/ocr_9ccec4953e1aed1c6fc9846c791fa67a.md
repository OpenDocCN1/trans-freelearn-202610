![](bbox=[0.25, 0.15, 0.75, 0.41])

Figure 12.8: Specifying primary object type and acceptable subtypes

We now prepare a Lifecycle State Diagram comprising four normal Lifecycle states:

+   - Start
+   - WIP
+   - Staging
+   - Approved

Mark the word normal that has been used to describe the Lifecycle states. There could also be exception states in a Lifecycle to represent an abnormal flow or diversions in the otherwise sequential and linear normal flow of the states.

You can introduce states in a Lifecycle by clicking on Add State and providing some unique understandable name for the state. Clicking on Add State adds a pentagon shaped state in the Lifecycle State Diagram as shown in figure 12.9.

The first state is called the **base state** while the end state is termed the **final state**. Our custom Lifecycle has four normal states, including a base state (called Start) and a final state (called Approved).

When you attach a document to a Lifecycle, you actually associate the document with the Lifecycle. Doing this allows you to promote the document to the next sequential linear normal state or demote it back to its previous normal state. Let us take a quick example.

We create a Word document abc.doc and associate it with our custom Lifecycle. At this stage, the document is in the WIP state of the Lifecycle. Promoting the document would push it to the next normal state—Staging. At the Staging state, if you demote the document, it will come back to WIP state. Simple, isn't it?