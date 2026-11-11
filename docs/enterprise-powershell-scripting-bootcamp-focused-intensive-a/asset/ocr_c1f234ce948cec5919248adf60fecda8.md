# 10

# Optimizing Script Execution Speed

When you execute the Windows server scanning script, you most likely will be processing a large number of objects. Small efficiencies in your coding practices can equate to exponentially faster execution times for large sets of data. Due to PowerShell being so flexible, it is possible to write two different sets of code that perform the exact same activity. However, you will find that certain coding techniques are much more efficient than others. In some cases, you can increase your script performance by over 200 percent.

This chapter focuses on the quick changes that you can make to your scripts to optimize efficiency. You will also learn about cmdlets and methods that you should avoid to ensure that you are not impacting your script's performance. By following the recommendations in this chapter, you can significantly reduce the amount of time that the Window server scanning script takes to scan systems.

In this chapter, you will:

+   - Learn how to measure the duration of commands in scripts
+   - Determine the overhead of writing all actions to the screen
+   - Create a logging function that allows you to turn on and off verbose logging
+   - Evaluate the write-progress function's impact on script performance
+   - Determine the speed differences between multiple `if` and `switch` statements