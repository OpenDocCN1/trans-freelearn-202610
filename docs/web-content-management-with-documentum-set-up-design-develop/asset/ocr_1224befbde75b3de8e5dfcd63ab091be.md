Click the Add Value Test button, choose attribute name and relational operator (=, <, >, <>, <=, and >=) and specify a value against which the attribute needs to be evaluated. Click on Add Expression Test..., choose an attribute and click on Validate followed by OK to introduce an expression that the system needs to check while entering this particular Lifecycle state.

![](bbox=[0.27, 0.23, 0.73, 0.57])

Figure 12.11: Lifecycle state editor Entry Criteria tab

You can even write a custom Docbasic procedure that the system can run after the attribute expressions have been evaluated. The custom procedure can programmatically evaluate certain conditions in the Docbase that must be true before a document enters this particular state in the Lifecycle.

You can optionally specify a Documentum out-of-the-box procedure by clicking on the ellipsis button against Docbase Path Name field and browsing to the procedure under the correct Docbase location.

6. Let's move over to the Actions tab in the Lifecycle state editor now. After the system has evaluated the entry criteria for a Lifecycle state, the set of actions to be performed by the system in the given Lifecycle state are specified within the Actions tab.

The actions are performed by Documentum in the following order of execution:

+   - The standard actions are evaluated first
+   - The optional Docbasic action procedure is run