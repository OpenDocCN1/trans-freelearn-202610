# Developing with AWS SAM

# Creating a New SAM Application

To initialize a new serverless application with AWS SAM, you must use the SAM CLI, which you will have to install. The Project Task section later in this chapter will guide you through the process in a detailed, step-by-step manner. You can then use the sam init code command to create a new SAM project with a predefined template and directory structure, helping you start quickly, as per the following example command:

```
sam init --runtime python3.9 --name todo-list-application
```

# Defining Resources in a SAM Template

To use the SAM service, you will need to create a SAM template within which you define your AWS resources, such as Lambda functions, API Gateway APIs, and DynamoDB tables. A SAM template is very similar in format to a CloudFormation template, which you learned about in Chapter 13, Building Infrastructure Using Code with CloudFormation and Cloud Development Kit (CDK).

The Transform and Resources sections are the only required components within a SAM template. A skeleton template to describe the anatomy of a SAM template is provided as follows:

```
Transform: AWS::Serverless-2016-10-31
Globals:
  set of globals
Description:
  string
Metadata:
  template metadata
Parameters:
  set of parameters
Mappings:
  set of mappings
Conditions:
  set of conditions
Resources:
  set of resources
Outputs:
  set of outputs
```

As part of your SAM template, you must declare the Transform section as Transform: AWS::Serverless:2016-10-31, as this identifies the template as a SAM template file.