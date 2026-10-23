# Project Task 8.4 – Create the VPC Link

In this project task, you will create a VPC link, a feature of API Gateway that enables secure connectivity to resources in a VPC:

+   1. Ensure you are still logged in to your AWS development account as the IAM user Alice.
+   2. From the top search bar, search for the term VPC links and select this service.
+   3. Click the Create button.
+   4. Select the VPC link for REST APIs option in the Choose a VPC link version section.
+   5. Provide a descriptive name, such as todolist-vpc-link.
+   6. Select the network load balancer you created earlier from the Target NLB search box.
+   7. Click the Create button.

Now that the VPC link has been created, you can set up and configure API Gateway.

# Project Task 8.5 – Integration with API Gateway

In this exercise, you will configure API Gateway:

+   1. Ensure you are logged in to your AWS development account as the IAM user Alice and are in the N. Virginia region.
+   2. From the search bar at the top of the management console, search for API Gateway and navigate to its console page.
+   3. Next, click the Build button in the REST API section. You will be directed to the Create REST API page.
+   4. Select the New API option.
+   5. Next, provide a descriptive name for your API, such as todo-list-api.
+   6. Click the Create API button.
+   7. You will be directed to the Resources section of the API Gateway console.
+   8. Click the Create Resource button. This will direct you to the Create resource page.
+   9. Ensure that you enable the Proxy resource toggle switch, which will ensure that it routes requests from any path to the network load balancer.

When using a proxy resource, you also need to define a greedy path parameter, ensuring you catch all of the resource’s sub-resources. For example, /GET/{proxy+} catches all the resources under GET/.