# Overall script structure

When you are creating scripts, you should adhere to a strict script structure. The structure of the script dictates the order of execution and how things are processed. The following diagram displays the proper method for organizing your script:

| Code | Description |
|------|-------------|
| `<# Comment Block #>` | ← Header |
| `Param($Input1,$Input2)` | ← Input Parameters |
| `$CPU = $env:computername` | ← Global Parameters |
| `Function doSomething { write-host "$cpu Done" }` | ← Functions |
| `doSomething` | ← Execution of Script |
| `write-host "Script Complete"` | ← End of Script |

It is recommended that you structure your scripts as follows:

+   1. **Declare the comment block:** To start the scripting process, declare a comment block and include everything that is pertinent to the script. This may include a description, revision information, author, editor, and additional notes.
+   2. **Declare the input parameters:** After the header, declare your input parameters, if required by the script. The input parameters are required to be directly after the comment block. Input parameters help the developers identify what is being inputted into the script, and what fields are required for proper execution of the script.
+   3. **Declare the global variables:** You should declare your global variables after the input parameters. Since variables declared in a function only stay within the boundary of the function, the variables you need to use globally should be defined here.
+   4. **Declare the functions in order:** The next portion of the structure is declaring your functions. If you have functions that call each other, you will want to declare the functions that are needed first in the script. This ensures that your scripts will not error out due to the function not being declared prior to execution.
+   5. **Start the execution of the script:** After declaring the functions, you can start the execution of the script. This section will call the other functions used by the global variables, and parameters to complete the tasks for the script.