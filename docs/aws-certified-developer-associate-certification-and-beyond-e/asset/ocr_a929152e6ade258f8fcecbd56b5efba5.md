Next, you must build a CI/CD pipeline for your backend application code.

# ## Project Task 12.6 – Create a CI/CD Pipeline for Your Backend Application

Now that your frontent code has been updated through AWS CodePipeline, you can create a pipeline for your backend code.

Before setting up the CI/CD pipeline, note the URI for the Docker image used to build that backend Fargate container. In a separate browser tab, navigate to your Amazon ECR management console. Make a note of the image URI of your backend Docker image, as per *Figure 12.48*. You will need this shortly in the upcoming steps.

![](bbox=[0.15, 0.33, 0.88, 0.6])

Figure 12.48 – Image URI of your Docker image

# *Create a CodeBuild Project*

+   1.  Navigate to the AWS CodeBuild management console in a separate browser tab.
+   2.  In the right-hand pane, click the **Create project** button.
+   3.  Provide a name for your project, such as `todolist-backend-build`.
+   4.  Next, select **AWS CodeCommit as Source provider** in the **Source** section.
+   5.  Select the `todolist-backend` repository from the **Repository** search box.
+   6.  Next, choose **main** from the **Branch** drop-down list.