+   14. Save the report by clicking File and then Save.
+   15. Give the report a Name and click the Save button.

You have now completed applying data science to your data.

# How it works

Figure 13.38 shows the artifacts created by this recipe:

![](bbox=[0.1, 0.22, 0.85, 0.6])

Figure 13.38: Artifacts created by this recipe

The first artifact created was of the Notebook type. Notebooks are used by data scientists to store informational narratives, code, and visuals. Essentially, notebooks are a way to experiment on data and document that experiment simultaneously. In Synapse Data Science, notebooks can contain either Python code or R code. Clicking the Run all button for the notebook runs all of the code blocks in sequence.

Next, we added a lakehouse and associated this lakehouse to the notebook. This allows the notebook to use the lakehouse as a repository for files and data. The second Python code block within the notebook loads the Superstore.xlsx file into the lakehouse. You can see the uploaded file within the lakehouse under Files.