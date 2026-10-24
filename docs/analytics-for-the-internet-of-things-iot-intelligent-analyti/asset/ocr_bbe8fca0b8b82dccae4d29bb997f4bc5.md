9. Verify that the bastion hosts have been deleted by going to **Services**, then **EC2**. The **EC2 Dashboard** should have a count of **0 Running Instances**. You can click on the **Running Instances** link to go to the EC2 list. The Linux bastion instances should either have a status of terminated or not be listed at all. If you are checking shortly after deleting the stacks, the instances will most likely still be visible in the list. As long as the status is terminated, you will not be charged:

![](bbox=[0.108, 0.243, 0.894, 0.433])

10. When recreating the environment, you will not need to recreate the key pair or the S3 bucket. Since the key pair and the S3 bucket were not part of the CloudFormation stack, it will remain in your environment after all the stacks are deleted. There is no charge for these unless you have data in the S3 bucket; even then, the costs are very low.

# Summary

In this chapter, we created a secure virtual private cloud in less than half an hour that is ready to support IoT analytics. We also reviewed how to delete and clean up the environment when no longer needed. We discussed AWS CloudFormation, NAT gateways, and Linux bastion hosts. You now have a secure and flexible place to launch large-scale analytics, which we will do later on in this book.