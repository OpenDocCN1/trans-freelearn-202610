![](bbox=[0.18, 0.09, 0.79, 0.63])

Figure 5.8 – Cloud Scheduler integration

The preceding screenshot is the Cloud Scheduler job that is generated from the Cloud Build manual trigger. Notice that Cloud Scheduler users the Cloud Build REST API to invoke the manual trigger.

# Summary

Cloud Build provides multiple ways to integrate with existing system tools and SCM platforms to participate in automated build pipelines. Cloud Build triggers provide the ability to execute a build pipeline. This can be through SCM events such as committing to a branch, external factors such as being the smaller part of a larger automation pipeline of another system tool, or at a specified time.

In this chapter, we showed an example of storing an SSH key in Secret Manager to be utilized by the build pipeline. In the next chapter, we will go further into security best practices with Cloud Build.