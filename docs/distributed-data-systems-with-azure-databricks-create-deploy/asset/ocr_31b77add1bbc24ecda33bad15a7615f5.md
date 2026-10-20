![](bbox=[0.17, 0.1, 0.8, 0.44])

Figure 3.31 – Using the parameter as a variable in the Azure Databricks job

10. Until this point, we haven't discussed how this variable will be consumed in the notebook. To do this, we will add a piece of code into our notebook. We will specify that this variable is the name of the file that we need to process and will use it to format the full path of the file to be read. The code that we will use is shown in the following snippet:

```
# Creating widgets for leveraging parameters
dbutils.widgets.text("input", "", "")
input_file = dbutils.widgets.get("input_file")
print ("Param -'input':")
print (input_file)
```

This way, we are using dbutils to create a widget that will look for a variable named input_file, which is the one we have specified in ADF.