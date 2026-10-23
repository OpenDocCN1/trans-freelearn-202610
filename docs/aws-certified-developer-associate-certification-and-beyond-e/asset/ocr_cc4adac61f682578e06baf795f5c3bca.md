34. Next, to test it, copy the URL and paste it into a browser tab. You should get a 200 response code, which means it works

```
{"result":{"status":"up","time":"Fri, 12 Jan 2024 19:35:16 GMT"},"statusCode":200}
```

At this stage, you have successfully deployed your API gateway, and the service connects to the backend application via the load balancers. The API Gateway has been configured to now provide access to backend APIs that are being hosted on the EC2 instances deployed in the private subnets. In the next chapter, you will bring all the components together to witness a fully functioning application by integrating both the frontend and backend components of our Todo List application architecture.

# Summary

This chapter explored serverless computing on AWS, focusing on the critical components like AWS Lambda and API Gateway. It was designed to impart practical skills and deep understanding, enabling you to build and manage serverless applications on the AWS platform effectively.

The chapter began with an overview of AWS Lambda, introducing the fundamentals of this serverless compute service. You learned about event-driven architecture and how Lambda functions can be triggered by various AWS services. This section laid the groundwork for understanding serverless computing paradigms and the operational efficiency of AWS Lambda.

Moving beyond the basics, the chapter delved into more sophisticated topics, such as environment variables, tagging for resource management, and running Lambda functions within a VPC. This advanced knowledge enabled you to optimize Lambda functions for security, cost-efficiency, and better resource management.

The focus then shifted to API Gateway. You were guided through the process of creating and managing REST and HTTP APIs. This section was crucial for understanding how to expose backend services securely and efficiently, dealing with topics such as API routing, data transformation, and securing APIs against unauthorized access.