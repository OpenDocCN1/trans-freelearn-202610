In the same menu where you can change the chart type, you also have the ability to set up an alert. Alerting allows for a certain action to be taken in case some predefined threshold on a metric is exceeded.

Here, we show the screen that is shown when you create a new alert:

![](bbox=[0.13, 0.18, 0.85, 0.42])

Figure 9.20 – Setting up an alert

We selected a CPU-based alert, stating that the alert should fire if the average CPU usage exceeds 85. We also stated that we want this alert to be checked every 5 minutes and that it also should look at the last 5 minutes to determine the value. Normally, you would want this period to be a bit longer to get a responsible idea about the application load and to prevent alert firing due to an incidental spike.

Having a number of these metrics on a dedicated dashboard for a container instance can be very useful, and luckily, we have the option to do so. Via the Save to dashboard option, you can add your favorite metrics to your personal or shared dashboard.

# Summary

In this chapter, we moved our application to a cloud platform. We chose Microsoft Azure, but it could have been any cloud provider. We showed how to set up a container registry, how to do the actual deployment, and how to look at metrics for a running container instance.

You have now moved your existing monolithic application to the cloud. You have enhanced it with cloud-native features, such as resilience. You have come a long way, and maybe this is where you intend to stop. Or, you may choose to go down the path of breaking up the monolith into individual applications or microservices. There is no good or bad here, and the choice of what to do depends entirely upon the application and its intended use. In the next chapter, we will look at MicroProfile as it can help add more cloud nativeness to your application.