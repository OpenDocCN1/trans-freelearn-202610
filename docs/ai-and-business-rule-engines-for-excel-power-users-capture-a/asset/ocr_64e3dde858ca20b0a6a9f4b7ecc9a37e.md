# More powerful business rule engines

So far in this chapter, we have described the problems you may have encountered using Excel, and how AI and, in particular, rules-based AI might be able to help solve these problems. These ideas are not very useful without a tool to implement them, and you will be glad to know that more powerful rule engines exist.

We will not go into too much detail (yet) on these rule engines, since we’ll start covering that in the next chapter. For now, we just want to prove that they exist, they are easy to use, and they follow the same when ... then format that we introduced earlier. Figure 1.7 gives an example of one more powerful business rule engine:

![](bbox=[0.13, 0.32, 0.88, 0.52])

Figure 1.7 – A guided business rule editor

Figure 1.7 shows a sample business rule written in the guided editor that we’ll see in Chapters 3 and 4. It uses the when ... then format that we’re now familiar with. The first three columns of the table are the when condition and the fourth is what action happens as soon as the condition is met.

Remember that this is a web-based editor, so it’s easy to use. These rules are saved in executable format, being ready to run in a rule engine.

Even without explaining the rule in detail, it is pretty easy to figure out what would activate it and then what will happen when it does. When a person resides in a particular location, then recommend them a particular chocolate (candy) bar.

> **Where is the information stored for our rules?**
> If you have an Excel background, where you’re used to referring to values in other cells such as =A4, you might wonder where values such as Country of Residence are coming from in the preceding example. For the moment, think of it like a named range in Excel – a convenient name to make things clearer.