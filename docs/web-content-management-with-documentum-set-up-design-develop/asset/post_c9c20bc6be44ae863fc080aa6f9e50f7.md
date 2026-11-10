
![](img/b18244e6621ecc39cef03ff6c0e55e7c_179_0.png)

Figure 12.6: Creating a new Lifecycle object

The Lifecycle Editor is used to create a **Lifecycle State Diagram** wherein you create the various states of a Lifecycle along with their specific behavior/attributes.

Note that the Validate button is disabled at this point of time. The Lifecycle is under creation and hence is said to be in the Draft state. When you have completed the creation of the Lifecycle and checked it into the Docbase, the Validate button gets enabled. Clicking on the Validate button amounts to system validation to check if the Lifecycle parameters are consistent or not. If the validation checks pass, the Lifecycle is Validated and the Install button gets enabled. After clicking on the Install button, the Lifecycle gets installed in the Docbase and is finally ready to be used by the appropriate objects.

### 2. Attach an Alias Set to the Lifecycle by clicking on the ellipsis button corresponding to the option Default Alias Set. The Select Alias Sets dialog box is shown as in figure 12.7.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_180_0.png)

Figure 12.7: Specifying the default Alias Set for the Lifecycle

Choose the Alias Set Custom_AliasSet we created earlier from the dropdown. You can additionally attach some other Alias Sets existing in the Docbase to the Lifecycle, by clicking on the Add... button.

### 3. Now you need to provide the object types, the object instances of which can be attached to the custom Lifecycle. Say for example, we require our custom Lifecycle to be attached to News Article objects.

Click on the ellipsis button corresponding to Primary Type field and choose dm_document as the Primary Type (refer to figure 12.8). This means that object instances of dm_document object types can be attachable to our custom Lifecycle. Further, the subtypes of the primary object type (dm_document in our case) are shown under the Available Subtypes list. Choose the subtypes of dm_document, the object instances of which can also be attached to the Lifecycle in question. Clicking on the → button will include the dm_document subtype under the Acceptable Subtypes list. Click the OK button.

Note that, as shown in the figure 12.8, we have specified dm_document as the primary object type of our custom Lifecycle and cust_newsarticle object type as the acceptable subtype of dm_document for the Lifecycle.

This ensures that content objects created from the News Article type can be attached to our custom Lifecycle.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_181_0.png)

Figure 12.8: Specifying primary object type and acceptable subtypes

We now prepare a Lifecycle State Diagram comprising four normal Lifecycle states:

- Start
- WIP
- Staging
- Approved

Mark the word normal that has been used to describe the Lifecycle states. There could also be exception states in a Lifecycle to represent an abnormal flow or diversions in the otherwise sequential and linear normal flow of the states.

You can introduce states in a Lifecycle by clicking on Add State and providing some unique understandable name for the state. Clicking on Add State adds a pentagon shaped state in the Lifecycle State Diagram as shown in figure 12.9.

The first state is called the **base state** while the end state is termed the **final state**. Our custom Lifecycle has four normal states, including a base state (called Start) and a final state (called Approved).

When you attach a document to a Lifecycle, you actually associate the document with the Lifecycle. Doing this allows you to promote the document to the next sequential linear normal state or demote it back to its previous normal state. Let us take a quick example.

We create a Word document abc.doc and associate it with our custom Lifecycle. At this stage, the document is in the WIP state of the Lifecycle. Promoting the document would push it to the next normal state—Staging. At the Staging state, if you demote the document, it will come back to WIP state. Simple, isn't it?

Although we will not be covering exception states in our custom Lifecycle, it is worth mentioning here that similar to promotion and demotion, moving from a normal to an exception state is termed **suspension** while moving back from an exception state to normal state is termed **resumption**.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_182_0.png)

Figure 12.9: A Lifecycle State Diagram

### 4. Double-click on the added state or select the state and click the Edit State button to open the Lifecycle state editor. We will take an example of creating the WIP state of the Lifecycle. Similarly, other states of the Lifecycle can be created using the Lifecycle editor.

Under the General tab (refer to figure 12.10), you can specify the name and description of the Lifecycle state and provide some state transition (state change) settings as follows:

- **Allow attachment directly to this state**: If the objects meet the Lifecycle state's entry criteria, this Lifecycle state will be their initial state.
- **Allow demotion to previous state**: This allows demotion from this state to its previous normal state. For the first state of the Lifecycle, this option is disabled.
- **Allow scheduled transitions from this state:** This allows Lifecycle state changes based on a time schedule without requiring any explicit action initiated by the user.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_183_0.png)

Figure 12.10: Lifecycle state editor General tab

- **Demote document to base state under these conditions:** This option allows demoting documents to the base state in the Lifecycle if the mentioned conditions are met (such as save, check-in, etc.).

### 5. Under the Entry Criteria tab (refer to figure 12.11), you can specify some conditions or checks that the system performs before a document enters a particular state in the Lifecycle.

Note that during demotion, Documentum does not check the entry criteria for the Lifecycle state.

The entry criteria checks can be performed via attribute values and expression tests. You can specify multiple conditions that are evaluated by Documentum from top to bottom of the list. Additionally, the system ensures that the moment one condition fails, the other conditions are not evaluated, thus saving execution time.

Click the Add Value Test button, choose attribute name and relational operator (=, <, >, <>, <=, and >=) and specify a value against which the attribute needs to be evaluated. Click on Add Expression Test..., choose an attribute and click on Validate followed by OK to introduce an expression that the system needs to check while entering this particular Lifecycle state.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_184_0.png)

Figure 12.11: Lifecycle state editor Entry Criteria tab

You can even write a custom Docbasic procedure that the system can run after the attribute expressions have been evaluated. The custom procedure can programmatically evaluate certain conditions in the Docbase that must be true before a document enters this particular state in the Lifecycle.

You can optionally specify a Documentum out-of-the-box procedure by clicking on the ellipsis button against Docbase Path Name field and browsing to the procedure under the correct Docbase location.

### 6. Let's move over to the Actions tab in the Lifecycle state editor now. After the system has evaluated the entry criteria for a Lifecycle state, the set of actions to be performed by the system in the given Lifecycle state are specified within the Actions tab.

The actions are performed by Documentum in the following order of execution:

- The standard actions are evaluated first
- The optional Docbasic action procedure is run

Clicking on the Add Action... button in the Actions tab shows the Define Action screen (refer to figure 12.12) allowing users to choose one of the numerous action types and define a specific action to be performed by the system. Defining an action is simple via a GUI wizard.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_185_0.png)

Figure 12.12: Define Action wizard under Actions tab

Using the Define Action wizard, you can specify multiple actions, such as adding a particular version label to or removing a particular version label from a document when it is in the given Lifecycle state.

You could additionally set the document's owner or a particular value for one of its attributes, apart from several other action definitions.

To add a WIP version label to a document when it is in the WIP state of the Lifecycle, choose the Add Version Label option from the Define Action wizard and enter the value WIP in the subsequent screen (refer to figure 12.13).
