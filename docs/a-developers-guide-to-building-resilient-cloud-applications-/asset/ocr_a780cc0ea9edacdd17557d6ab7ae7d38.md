# Set properties

Name

InputDS

Linked service *

AzureDataLakeStorage

File path

healthcareadf / input / inputfile.txt

Figure 10.13 – The Set properties page of a dataset for the input

+   5. Create another dataset following the same steps as before, but specify this as an output. Select OK to confirm this:

# Set properties

Name

OutputDS

Linked service *

AzureDataLakeStorage

File path

healthcareadf / output / File name

Figure 10.14 – The Set properties page of a dataset for the output

After creating the dataset, we will create our pipeline with a copy activity using the input and output datasets. This copy activity will copy our data from the input dataset file to the output dataset file settings.

# Creating a pipeline

To create a pipeline, we will follow these steps:

+   1. Again, select the plus (+) button, then Pipeline | Pipeline, as presented in the following figure: