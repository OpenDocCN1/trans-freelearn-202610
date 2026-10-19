# Adding durability features

While creating the Electroniz pipelines, we created several types of activities to perform specific actions. Looking closely at the properties of each activity, you will notice three important ones that can add durability to your pipelines:

![](bbox=[0.21, 0.2, 0.75, 0.64])

Figure 9.14 - Durability properties in an activity

The following list explains the parameters:

+   - **Timeout**: This property represents the maximum amount of time a particular activity should run for. In the Azure data factory, the value is set to 7 days by default. After the timeout expires, the activity is terminated whether it was successful or not. Now that might sound very bad, but it is not. On the contrary, it is highly advisable to set a timeout value to a safe value. With some initial monitoring performed during the development and testing phases, it is easy to predict how much time a particular activity usually takes. For instance, you might have a notebook activity in your pipeline that usually takes 5 minutes to finish. If you add a buffer, you might want to set the value to 15-30 minutes.