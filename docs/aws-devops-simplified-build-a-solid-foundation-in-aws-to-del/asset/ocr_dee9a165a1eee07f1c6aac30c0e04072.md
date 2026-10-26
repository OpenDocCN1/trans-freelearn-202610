![](bbox=[0.14, 0.09, 0.85, 0.38])

Figure 8.5 – Grafana login and data source configuration

This option leads to the data source configuration page, where you can add additional details of the Prometheus workspace you want to integrate with. Assuming that you haven’t created any other workspace in the same account, you will find the one that was provisioned by the CDK stack, after you select the appropriate AWS region, which in our case is always eu-central-1, or Frankfurt.

If you see multiple workspaces, please reference the identifier for the one that was created as part of the CDK deployment. This should be available on the recently deployed CloudFormation stack details page.

Once you’ve filled in all the data source configuration fields for Prometheus, you should see something similar to the following: