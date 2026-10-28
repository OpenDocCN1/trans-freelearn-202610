When you are done, click on **Copy**. All items should be successfully copied.

> In this case, the destination template did not have any items configured. As it is not possible to have two items for a single host with the same key, attempting to copy over an already existing item would fail.

In the upper left corner, click on the **Details** link. That expands the messages, and we can see that all of these items were added to the target template:

![](bbox=[0.07, 0.285, 0.92, 0.345])

Now we have to do the same with triggers, so click on **Triggers** in the navigation bar above the item list, then click the checkbox in the header. This time uncheck the **One SSH service is down**, because this trigger spans both hosts. If we copied this trigger to the template, that would create all kinds of weird effects.

> The sequence here, copying items first, then triggers, was important. A trigger cannot be created if an item it references is missing, so attempting to copy triggers first would have failed. Copying a trigger will not attempt to copy the items the trigger is referencing.

Again, click on the **Copy** button at the bottom. In the next screen, choose **Templates** in the **Target type** dropdown and **Custom templates** in the **Group** dropdown. Mark the checkbox next to **C_Template_Linux** in the **Target** section, then click on **Copy**. All triggers should be successfully copied. Of course, we don't have to create a host first, create entities on it, then copy them to a template — when creating a fresh template, you'll want to create entities on the template directly. If you have been less careful and haven't thought about templating beforehand, copying like this is a nice way to create the template more quickly.