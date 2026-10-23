You must terminate these instances by selecting them and then from the Instance state drop-down list, click Terminate instance as per the following screenshot:

![](bbox=[0.145, 0.135, 0.87, 0.225])

Figure 9.14: Terminate existing instances to force autoscaling services to deploy new ones

+   8. You will be prompted to confirm the termination of these instances. Go ahead and click Terminate in the dialog box that appears.

+   9. Once these instances have been terminated, the Auto Scaling service will launch new instances using the updated application code from your Amazon S3 bucket acting as the repository. This will usually take a few minutes. Click on Auto Scaling Groups from the left-hand menu in the Auto Scaling section. From the right-hand pane, select todo-list-auto-scaling-group, which you created in Chapter 7, Building Application Solutions with High Availability, Elasticity, and Data Security. In the right-hand pane, click Activity. You will find that the two previous instances were deleted and replaced with two new instances, as per the following screenshot:

![](bbox=[0.145, 0.435, 0.87, 0.62])

Figure 9.15: Auto Scaling group launches two new instances in response to the previous step’s termination action of existing instances

In the next task, you will update the frontend website code. This new version includes the necessary integration with Amazon Cognito to enable users to sign up and sign in to the application.