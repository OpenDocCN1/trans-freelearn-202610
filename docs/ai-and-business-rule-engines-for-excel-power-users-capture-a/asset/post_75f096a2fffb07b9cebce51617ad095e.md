
## Option 2 – step-by-step instructions

Our step-by-step instructions start with the Hello World Decision Model that we last saw in Figure 3.20. We’re going to make the following changes to it, to give us a Decision Model like in Figure 3.24:

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_89_0.png)
Figure 3.24 – Adding a second data node

To do this, follow these steps:

- 1. Double-click on the Decision Model name, and rename it to Good Morning.
- 2. Add a new Data node to the diagram (drag and drop from the menu on the left).
- 3. Double-click on this Data node and rename it AM.
- 4. Select the AM Data node, click on the Arrow node that appears, then drag the arrow to connect it to the greeting decision node. Be careful of the arrow direction – it should go from the AM node to the greeting node.
- 5. Select the AM Data node, and then click on the Properties icon (top right of the screen), to show the Properties editor panel.
- 6. Using the Properties editor, change Data type for the AM Data node to boolean.

## Boolean data types

You’ve probably used Boolean data types in Excel – they are either True or False. Like in Excel, there is a third Boolean type of empty, where no value has been supplied.

Sometimes, the impact of an empty value shows up as a Required Dependency ... not found message, which disappears when we pass in a value. Keep this in mind as we run the sample.

Now that we are supplying additional information to our decision node, we want to update our decision to take advantage of it. To do this, follow these steps:

- 1. Select the greeting Decision node in our Decision Model.
- 2. Click on the Edit button that appears on the middle left as a context menu.
- 3. Using the boxed editor, update the expression to if AM then "Good Morning "+name else "Good Afternoon "+name.
- 4. The result should look like Figure 3.25 – note that the editor has captured the text, but won’t show all of it on screen unless we drag the right-hand side of the boxed editor to expand it:

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_90_0.png)
Figure 3.25 – Updating the decision expression

- 5. Click on the Problems tab at the bottom left of the screen to see whether KIE has highlighted any issues. A major source of errors is getting the characters for the quotes around the text wrong – it’s the same double-quote character you use in Excel.

No doubt you’re keen to try out these changes. Like a previous example, click on the Run … as Form menu at the top right of the screen and enter some values like in Figure 3.26:

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_90_1.png)
Figure 3.26 – Updated input/output form

On the **Inputs** side, you’ll see that our two inputs are automatically listed – a textbox for entering a name and a checkbox for our Boolean **AM** value. Like before, when we update the values, the decision model evaluates them and outputs a value – in this case, a time-appropriate greeting.

In our Decision node, the expression box used a FEEL expression (**If** ... **Else**) to decide whether to say **Good Morning** or **Good Afternoon** based on whether the **AM** box is checked or not.

> **What is greeting (null)?**
> You may have noticed that before you input any values into the form, there may be a message in the output, something such as **greeting (null)**.
> 
> This is because of the empty value in our Boolean – because we pass in an empty value, KIE doesn’t have enough information to make a decision. Rather than KIE hiding this issue, it tells us by passing back the null message.

This is still a relatively simple example – we’re only scratching the surface of the FEEL expression language. We haven’t yet begun to chain multiple decision nodes to make more complicated decisions. And we haven’t yet introduced decision tables that split our business decisions into very simple **when ... then** rules. But you can already begin to see the power of taking something normally crammed into a hard-to-read Excel expression and editing it as a much clearer Decision Model.

We’ll cover all of those items in the next chapter. But before we get onto more powerful tools, it’s worth completing our tour of KIE Sandbox, as there are several other features that we will find useful.

## Other UI elements in KIE Sandbox

On the Decision Model page of KIE Sandbox, if we click on the *Decision Navigator* icon (the flag-like icon at the top right), KIE will show **Decision Navigator**, like in Figure 3.27:

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_91_0.png)
Figure 3.27 – Decision Navigator view

The **Decision Navigator** view is another layout to represent the same Decision Model – this time, as a tree-like structure. All the nodes are the same, but it is easier to see the content of the nodes (for example, when we use a literal expression to make our decision). Clicking on the elements in **Decision Navigator** will jump to the relevant part of the diagram.

Also on the right-hand side of the screen is **Explore diagram**. The icon looks like an eye, as highlighted in Figure 3.28:

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_92_0.png)
Figure 3.28 – Explore diagram

**Explore diagram** is really useful for exploring larger decision models – which is why Figure 3.28 shows the more complicated sample Decision Model that comes with KIE. While you can’t read the text on the individual nodes, it allows you to zoom out and see the bigger picture.

Returning to the Decision Model editor, just underneath our model name, you’ll see four tabs, as shown in Figure 3.29:

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_92_1.png)
Figure 3.29 – Documentation tab

Let’s look at the tabs:

- **Editor** is the visual decision model editor that we’ve been working with throughout this chapter.
- **Documentation** summarizes our decision model into an easy-to-export format (such as PDF). We can share these PDF docs with the wider team and check for agreement on business decisions.
- **Data Types** allows us to build more complex data types, which we will explore in the next chapter. We touched on these in our examples.
- **Included Models** allows us to include other decision models that we can use to build our business decision. We don’t need to cram our decisions into one Decision Model.

Finally, the down arrow beside our model name at the top is a shortcut that allows us to quickly navigate between other Decision Models we have created.

## Note – KIE Sandbox is only a sandbox

To be more precise, KIE Sandbox Extended Services is only a development tool. It’s easy to be impressed by the fluid nature of editing decision models, and you should be! The KIE Sandbox editor is one of the best, most robust implementations of the OMG Decision Model standard and shows the time and effort that the IBM and Red Hat teams have put into it.

However, please remember the runtime (KIE Sandbox Extended Services) is only a convenience to help you evaluate your decision models as you build them. It can only run on your local machine, accessible only to you, so it’s not suitable for deploying as a stable solution. In *Chapters 5 and 10*, we will show you how to run your business rules in a truly robust, scalable, enterprise solution.

## Summary

We achieved a lot in this chapter – we went from talking about business rules as an abstract concept, to executing business rules using hands-on tools such as KIE Sandbox.

Along the way, we saw how Decision Models are another way to represent our business rules, and their highly visual format meant that we could quickly build two samples. We downloaded and ran KIE Sandbox Extended Services to evaluate our Decision Model and saw some of the different data types on which we can base our decisions.

We took a tour of the UI to understand how we can import and export decision models, and where to access previous models that KIE has autosaved for us. We also saw some of the other views of Decision Models, which are especially useful when sharing them with colleagues for validation.

However, there were several key points in the chapter where we understood Decision Models, rule engines, and the FEEL expression language are much more powerful than we’ve previously explored. Let’s stay within KIE Sandbox for our next chapter and do a deep-dive to explore the power these tools give you.
