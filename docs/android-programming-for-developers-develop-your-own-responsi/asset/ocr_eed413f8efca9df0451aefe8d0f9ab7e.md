Notice the top element of the **Component tree** window, after the **Device Screen** element. We already have `RelativeLayout` by default. This type of layout allows its children to use descriptions of positions relative to itself and its other children. Let's explore this:

+   1. Left-click and drag a **Plain TextView** widget from the palette and drop it just underneath the existing **Plain TextView**.\n2. Now do the same with **Button** and drop it below the `TextView` we added in the previous step.\n3. Next place a **Switch** widget below the **Button** widget from the last step.

Your layout will probably look something like this next screenshot, which is a close-up view of the top left of the UI designer:

![](bbox=[0.112, 0.406, 0.281, 0.526])

What is more interesting than the appearance, however, is the XML code that has been generated. Let's explore it now. Left-click on the **Text** tab below the editor window to reveal the generated XML.

Here are the entire contents of `activity_layout_experiments.xml` with the code for the parent `RelativeLayout` removed. Take a look at the code and take a close look at the three highlighted lines:

```
<TextView\n    android:text="@string/hello_world"\n    android:layout_width="wrap_content"\n    android:layout_height="wrap_content"\n    android:id="@+id/textView" >\n\n<TextView\n    android:layout_width="wrap_content"
```