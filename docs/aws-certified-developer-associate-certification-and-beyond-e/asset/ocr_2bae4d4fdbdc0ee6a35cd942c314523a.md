The key topics covered in this chapter are as follows:

+   - Using AWS Serverless Application Model (SAM) to develop and deploy serverless applications
+   - Overview of AWS AppSync
+   - Project task – deploying the to-do list applications ‘coming soon’ page using SAM

By the end of this chapter, you will not only grasp the fundamentals and advanced features of these serverless technologies but also understand their practical application in building and scaling applications without managing the underlying infrastructure. This approach enhances agility and focuses on cost efficiency and performance, which is crucial for today’s fast-paced software environments.

# Using AWS Serverless Application Model (SAM) to Develop and Deploy Serverless Applications

# Overview of AWS SAM

AWS SAM is an open source framework designed specifically for building serverless applications on AWS. It provides a simplified definition of the serverless infrastructure needed in AWS CloudFormation syntax. AWS SAM is an extension of CloudFormation and provides a shorthand syntax to express functions, APIs, databases, and event source mappings. With SAM, you can define your serverless application with just a few lines of code in a YAML configuration file.

# Key Components of AWS SAM

AWS SAM primarily consists of three primary components:

+   - **SAM template:** A configuration file that follows the YAML format. This file defines the resources in your serverless application using the SAM syntax. You use the template to declare the resources you want to provision for your serverless application. The resources you would declare in a CloudFormation template can also be declared in a SAM template.
+   - **SAM CLI:** A CLI that provides tools to build, test, and deploy your AWS SAM applications.
+   - **Deployment methodology:** AWS SAM integrates with AWS CloudFormation, leveraging its capabilities to deploy serverless applications as stacks and manage them throughout their life cycle.

The next provides an overview of developing your application with SAM.