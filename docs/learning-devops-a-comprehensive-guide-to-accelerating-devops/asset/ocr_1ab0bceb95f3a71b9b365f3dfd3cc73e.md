3. Then, commit this new GitHub Actions workflow file. This will automatically trigger the SonarCloud analysis of this project.

4. After a few minutes, you will see the result of two executions. First, you will see that the GitHub Actions workflow has succeeded:

![](bbox=[0.131, 0.192, 0.867, 0.34])

# Figure 16.28 - GitHub Actions Sonar analyze

Then, you will see the analysis in the SonarCloud dashboard, as shown in the following screenshot:

![](bbox=[0.131, 0.422, 0.867, 0.71])

# Figure 16.29 - SonarCloud analysis results

So, with each new code commit on this repository, either directly or via merging a pull request, code analysis will be triggered and the SonarCloud dashboard will be updated.

It is clear that our end goal is to integrate SonarCloud analysis into a CI/CD pipeline, so here are some resources to help us integrate it:

+   • If you’re using Azure DevOps, here is a complete tutorial that will help you integrate SonarCloud into the pipeline: `https://docs.microsoft.com/en-us/labs/devops/sonarcloudlab/`.