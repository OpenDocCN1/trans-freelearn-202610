![](bbox=[0.15, 0.1, 0.87, 0.27])

Once all the values are stored in the table, you can verify whether they are loaded correctly by choosing a DynamoDB table name and then selecting **Explore table items**, which will take you to this screen:

![](bbox=[0.15, 0.36, 0.87, 0.56])

Now we have our queries ready in the DynamoDB table to be executed, we will see how we can execute them through a step function workflow in a serverless manner.

# Serverless data quality pipeline

Navigate to **Step function** in the AWS console, and create a state machine. Choose the **Standard** workflow and choose to develop visually, as shown here:

![](bbox=[0.15, 0.74, 0.87, 0.9])