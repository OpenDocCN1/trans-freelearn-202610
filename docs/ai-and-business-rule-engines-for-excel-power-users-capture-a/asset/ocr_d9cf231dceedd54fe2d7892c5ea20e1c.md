• Mainly, you’ll want to edit the rules files. *Figure 12.11* shows the DRL rules file for the second Red Piranha example open in the editor:

![](bbox=[0.131, 0.156, 0.853, 0.454])

Figure 12.13 – DRL rules file for the second Red Piranha example in VS Code

This rule file is in the DRL format, which KIE also supports. Since DRL is a more advanced rules format, let’s go through this rules format in detail in the next section.

# Advanced DRL rules in VS Code and Business Central

When we first introduced business rules in *Chapter 1*, we used a slightly formal when … then format – when something is true, then do this action. The rule in our Red Piranha example in *Figure 12.11* follows this format exactly:

+   - **When** a Cell object is marked as unmodified
+   - **Then** update the status flag to modified/print a message to the console

So, the basic format is simple and very human-readable. There is a lot of power available when writing individual rules this way, and DRL files can be mixed and matched with the decision models that we’ve been using for most of this book.

> **Each line in a decision table is equivalent to one DRL rule**
> 
> We described decision tables as having columns equivalent to when (conditions) and columns that activate when matched (then, on the right-hand side of a decision table). So, you can think of each line of a decision table as being equivalent to one rule in a DRL file.