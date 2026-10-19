+   3. Click the **Run** button in the main pane. The console should appear at the bottom of the side pane with **Hello World**, like in *Figure 7.9*:

![](bbox=[0.26, 0.158, 0.76, 0.236])

Figure 7.9 – Hello World result

While it’s only a Hello World example, we’ve seen the structure of a standard Script Lab project. That structure won’t change – which makes our next step, calling our decision service from Script Lab, much easier to write.

# Calling our decision service from Script Lab

Since the Hello World project follows the standard Script Lab format, and our calls to the decision service are also in a standard format, it is no surprise that we will be familiar with much of the script when we combine the two to call our decision service from Script Lab in Excel.

Rather than repeat a lot of the familiar elements, we suggest you download the sample from the GitHub book site – it’s 07_Script_Lab_Decision_Service.yaml. Like before, open this text in an editor, copy all of it, then open the hamburger menu in the top left of Script Lab, select **Import**, and paste all of the text.

That way, we can focus on the interesting parts in the following walk-through.

There are several elements of the user interface defined in this file. Take a look at the **HTML** tab in Script Lab and you can see the input for Customer Number, as in the following extract. The other text fields follow a similar pattern:

```
<div class="ms-TextField">Customer Number
  <input type="number" id="cNumber" value="update me"/>
</div>
```

Together, this and the other HTML elements give a user interface as in *Figure 7.10*. To keep things simple, we haven’t included fields for all the information we could possibly pass to our decision service – but it’s trivial to add them using the same pattern if you want.