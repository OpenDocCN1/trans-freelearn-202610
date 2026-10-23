To ensure no reduced availability, in *Step 2*, an additional batch of two nodes is introduced, bringing the total number of nodes to six. In *Step 3*, the first two nodes are upgraded, taking them offline. However, the number of nodes serving end users remains at four.

In *Step 5*, the next set of two nodes are upgraded. While these two nodes are taken offline for the upgrade, the same number of available nodes exists in the fleet and this avoids reduced availability. Once the upgrade is complete, you will have six nodes (including the two additional ones added for the deployment strategy).

Finally, the additional batch of nodes is deleted once all existing nodes have been upgraded to the latest application version.

In this example, you will incur additional costs for deploying the additional batch to facilitate this deployment strategy. In addition, the upgrade process is longer than the previous batch. However, you benefit from the fact that end users do not experience performance issues or reduced availability of services.

# Immutable Deployment

As an alternative to rolling deployment, this strategy focuses on provisioning new resources in a separate autoscaling group to test the new application and, if necessary, roll back by simply deleting the autoscaling group. This will ensure that your existing fleet is not affected, and the existing instances are not reduced in availability.

With this deployment strategy, Amazon Elastic Beanstalk will create a second autoscaling group and provision a single instance to test the new configuration and application code. Once the launch has been successful, Amazon Elastic Beanstalk will provision additional instances to match the original autoscaling group. When all instances have passed the necessary health checks, Elastic Beanstalk will move the instances into the original autoscaling group and terminate the temporary autoscaling group and the old instances.

The following points should be noted about this deployment strategy:

+   - During the update process, the total capacity of your environment doubles for a short period. This is when the new autoscaling group serves requests before the original autoscaling group’s instances are terminated, resulting in additional costs. Note that this can also affect your quotas and limits so you will need to ensure you do not cross any such limits during the update process.
+   
+   - An immutable update results in provisioning new instances for every new version that you deploy. You may need to ensure that you use the correct version of the **Amazon Machine Image (AMI)** used to launch your instances with any necessary modifications to your AMI image as a precursor to the rollout.