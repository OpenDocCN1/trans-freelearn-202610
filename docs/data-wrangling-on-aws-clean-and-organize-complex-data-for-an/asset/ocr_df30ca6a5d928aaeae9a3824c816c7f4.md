+   7. Select the newly created data quality ruleset and click on **Click profile job with ruleset**.
+   8. Follow the previously described steps to create a new profile job.

![](bbox=[0.145, 0.165, 0.87, 0.335])

Figure 2.63: Data quality rulesets list in AWS Glue DataBrew console

Once it completes, you will be able to validate the data quality. You will see that all the rules have passed except **Check total record count** because you removed the duplicate values.

![](bbox=[0.145, 0.43, 0.87, 0.735])

Figure 2.64: Applying the recipe on the dataset