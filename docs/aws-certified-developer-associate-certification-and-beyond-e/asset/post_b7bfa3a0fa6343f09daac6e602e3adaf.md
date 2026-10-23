
## Update Your IAM Role

- 1. Navigate to the AWS IAM management console in a separate browser tab.
- 2. In the IAM console, from the left-hand menu, click on **Roles**, and then in the right-hand pane, search for the IAM role name you made a note of in *Step 16* earlier.
- 3. Click on the role, and from the **Add Permissions** drop-down arrow, click **Create inline policy**. For this inline policy, you can use the visual editor or the **JSON** tab and paste in the following JSON code, ensuring that you provide the correct values for the placeholders highlighted in bold:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
                "s3:GetObject",
                "cloudfront:CreateInvalidation"
            ],
            "Resource": [
                "arn:aws:cloudfront::[Account ID]:distribution/[CLOUDFRONT DISTRIBUTION ID]",
                "arn:aws:s3:::[AMAZON S3 BUCKET NAME]/*"
            ]
        }
    ]
}
```

> **Note:**
> 
> You can download this policy sample from our GitHub repository which is mentioned in the technical requirements section

- 4. Click **Next**.
- 5. Provide a name for your policy in the textbox labeled **Policy name** and then click **Create policy**.
- 6. You should have two policies for this role, as shown in *Figure 12.44*.

![](img/34ff9b5b5a734dae3450ab701e223c73_541_0.png)

Figure 12.44 – Additional inline policy to grant access to S3 and CloudFront

Next, you will be creating the pipeline for your CI/CD deployment.

## Create Your CI/CD Pipeline

In this section of the task, you will be creating a CI/CD pipeline, which will be used to trigger the deployment of your updated frontend code:

- 1. In the top search bar of the AWS Management Console, search for and navigate to the AWS CodePipeline console in a separate browser tab.
- 2. From the left-hand menu, select Pipelines under CodePipeline.
- 3. In the right-hand pane, click the Create pipeline button.
- 4. Provide a descriptive name for your pipeline, for example, todolist-frontend-pipeline.
- 5. Ensure that Queued (Pipeline type V2 required) is selected under Execution mode and then click Next at the bottom of the page. Note that this procedure will also create an IAM role automatically.
- 6. Select AWS CodeCommit as Source provider in the Source section of the page.
- 7. Next, ensure that you select the todolist-frontend repository for Repository name.
- 8. Next, ensure that you select main under Branch name.

9. Click Next, as per Figure 12.45:

![](img/34ff9b5b5a734dae3450ab701e223c73_542_0.png)

Figure 12.45 – Configuring the source of your pipeline

10. For Build provider, ensure that you select AWS CodeBuild.

11. Next, ensure that the region is set to US East (N. Virginia).

12. Next, ensure that you select the correct project name of the build that you created earlier in Step 12.

13. Click Next.

14. You will now be presented with Add deploy stage. For this stage, click the Skip deploy stage button. You will be prompted to confirm your choice. Go ahead and click the Skip button.

15. Next, you are presented with the Review section. Ensure that all the settings have been defined correctly, as per the previous steps, and then click the Create pipeline button.

Your AWS CodePipeline will now initialize and proceed to build out your CI/CD pipeline, taking in the updated code in the repository and updating the frontend components of your web application. Within a few moments, you should see that the pipeline is successfully created and your source and build processes are also completed successfully, as per Figure 12.46:

![](img/34ff9b5b5a734dae3450ab701e223c73_543_0.png)

If you navigate to the contents of the Amazon S3 bucket hosting your frontend website code, you will note that the files have been updated with the timestamps, as per Figure 12.47:

![](img/34ff9b5b5a734dae3450ab701e223c73_543_1.png)

Next, you must build a CI/CD pipeline for your backend application code.

## Project Task 12.6 – Create a CI/CD Pipeline for Your Backend Application

Now that your frontend code has been updated through AWS CodePipeline, you can create a pipeline for your backend code.

Before setting up the CI/CD pipeline, note the URI for the Docker image used to build that backend Fargate container. In a separate browser tab, navigate to your Amazon ECR management console. Make a note of the image URI of your backend Docker image, as per *Figure 12.48*. You will need this shortly in the upcoming steps.

![](img/34ff9b5b5a734dae3450ab701e223c73_544_0.png)

Figure 12.48 – Image URI of your Docker image

### Create a CodeBuild Project

- 1. Navigate to the AWS CodeBuild management console in a separate browser tab.
- 2. In the right-hand pane, click the **Create project** button.
- 3. Provide a name for your project, such as `todolist-backend-build`.
- 4. Next, select **AWS CodeCommit as Source provider** in the **Source** section.
- 5. Select the `todolist-backend` repository from the **Repository** search box.
- 6. Next, choose **main** from the **Branch** drop-down list.
- 7. Next, in the **Environment** section, note the **Role name** value that will be created as the new service role.
- 8. Scroll down and expand the **Additional configuration** subsection.
- 9. In the **Environmental variables** section, create a new environment variable called `REPOSITORY_URI` and, for the value, paste in the Docker image URI you noted at the beginning of this project task, as per *Figure 12.49*. The docker build command in the `buildspec.yml` file references this environmental variable to know which container image to build.

![](img/34ff9b5b5a734dae3450ab701e223c73_545_0.png)

Figure 12.49 – Environmental variables used in your CodeBuild project

- 10. Next, scroll down till you get to the **Buildspec** section. Here, select the **Use a buildspec file** option.
- 11. Finally, click the **Create build project** button at the bottom of the screen. You should receive a message stating that you have successfully created the project.

## Update Your IAM Role

- 1. Navigate to the IAM management console in a separate browser tab.
- 2. From the left-hand menu, click on **Roles**.
- 3. In the right-hand pane, search for and click to access the role you noted in *Step 7*, which CodeBuild automatically created.
- 4. Click the drop-down arrow on **Add permissions** and select **Create inline policy**.
- 5. Select the **JSON** option in **Policy editor** and paste in the following policy, making sure to replace the region name and the ECR image name in the ARN of your repository so that it matches the one in your Amazon ECR repository:

```
{
"Version": "2012-10-17",
"Statement": [
{
"Sid": "VisualEditor0",
"Effect": "Allow",
"Action": [
```

```
"ecr:CompleteLayerUpload",
"ecr:UploadLayerPart",
"ecr:InitiateLayerUpload",
"ecr:BatchCheckLayerAvailability",
"ecr:PutImage"
],
"Resource": "arn:aws:ecr:[REGION NAME]:[Account ID]:repository/[ECR IMAGE NAME]"
},
{
"Sid": "VisualEditor1",
"Effect": "Allow",
"Action": "ecr:GetAuthorizationToken",
"Resource": "*"
}
]
}
```

> **Note:**
> You can download this policy sample from our GitHub repository which is mentioned in the technical requirements section

- 6. Click **Next**.
- 7. Provide a name for your policy such as todolist-backend-policy-ecr.
- 8. Click the **Create policy** button.
- 9. Next, navigate back to the AWS CodeBuild console. Click on the **Start build** button, as per Figure 12.50:

![](img/34ff9b5b5a734dae3450ab701e223c73_546_0.png)

Figure 12.50 – Start build to test the build

10. If the build is successful, you will note the details in the **Build logs** section of your build project. If you navigate to your ECR management console, you will also note a new container image version with the latest commit hash, as per Figure 12.51:

![](img/34ff9b5b5a734dae3450ab701e223c73_547_0.png)

Figure 12.51 – New image version in Amazon ECR

Next, you can proceed to build the CI/CD pipeline.

## Create your CI/CD Pipeline

- 1. Navigate to the AWS CodePipeline management console in a separate browser tab.
- 2. In the right-hand pane, click the **Create pipeline** button.
- 3. Provide a name for your pipeline such as `todolist-backend-pipeline`.
- 4. Next, accept the default values and click the **Next** button.
- 5. For **Source provider**, choose **AWS CodeCommit**.
- 6. Next, select your backend repository from the **Repository name** search box.
- 7. Ensure that you set **main** as **Branch name**.
- 8. Click **Next**.
- 9. Choose **AWS CodeBuild** as **Build provider**.
- 10. Ensure that **US East (N.Virginia)** is selected for **Region**.
