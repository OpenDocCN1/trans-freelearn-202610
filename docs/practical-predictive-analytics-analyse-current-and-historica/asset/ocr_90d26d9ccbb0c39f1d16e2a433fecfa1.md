# Running the code

After loading the notebook, a series of code 'chunks' will appear. This particular notebook is an R type notebook, that is, by default, code lines can be R commands, SparkR commands, or special Databricks commands (such as display).

We will see later that we can have different cells for SQL commands (notebooks starting with %sql) or even intermix Python (%python) or scala code (%scala).

to run the contents of any particular code chunk you can use the following keyboard sequence:

```
<Shift + > <Enter> : Run command and move to next cell
```

Alternatively, use can use the run cell icon (the first triangle icon):

![](bbox=[0.112, 0.536, 0.242, 0.563])

Now that we now how to run individual code chunks, we will begin by running the initialization code which simply tests the Databricks system to see if it is operational and also sets some options.