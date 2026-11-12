Clicking on **Create Test Bench** will open it in a test bench workflow. The Test Bench workflow is a temporary sequence that's not part of the current project and is discarded when closed:

![](bbox=[0.242, 0.172, 0.761, 0.442])

Figure 13.40 – A temporary test bench workflow created

The selected activity is automatically added to a new temporary sequence file not included in your project. You can edit the properties of this activity or add new activities and later debug the test bench workflow.

+   2. To save the file, use the **Save as** option in the **DESIGN** ribbon bar to add a filename, and save it to the same file path as your project. Later, you can use the **Invoke Workflow File** activity and call the sequence in the desired workflow.

> > **Note**
> > **Create Test Bench** does not work with the `Pick Branch` activity because it requires the `Pick` activity as a container to execute.

# Run to this Activity

Let's say, for example, you have five activities inside a workflow, and you want to run till the second activity only. Then, choose the **Run to this Activity** option from the context menu by selecting the third activity.