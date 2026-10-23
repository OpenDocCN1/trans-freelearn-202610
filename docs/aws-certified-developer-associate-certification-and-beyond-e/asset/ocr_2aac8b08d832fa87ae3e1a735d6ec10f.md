# Conditions

Conditions allow you further flexibility in the deployment of stacks. You can define variations in your deployments based on whether a condition is validated as true or false. You might want a stack configured in a particular manner if your deployment is within a development versus a production environment.

For example, you can define an **EnvType** input parameter, which will have a default value but also allow you to input from a choice of dev, test, or prod. You can also use pseudo parameters to define your predefined conditions.

Next, you can define a Conditions section in your template that uses intrinsic condition functions to determine whether the associated resource is created. Finally, you can associate a condition with the resources or output that you want to create based on the condition being true. If the condition validates to false, then the resources or output will not be created.

When defining your conditions, you must use one of the following conditions’ intrinsic functions to validate it:

+   - Fn::And
+   - Fn::Equals
+   - Fn::ForEach
+   - Fn::If
+   - Fn::Not
+   - Fn::Or

> **Note**
> When using the YAML-formatted template, the syntax of the intrinsic functions is slightly different. For example, Fn::Equals in JSON is written as !Equals in YAML.

In the following code snippet, note that a condition has been set such that if an EC2 instance is deployed in a prod environment, it will be configured with an EBS volume:

```
AWSTemplateFormatVersion: "2010-09-09"
Parameters:
  EnvType:
    Description: Environment type.
    Default: test
    Type: String
    AllowedValues: [prod, dev, test]
    ConstraintDescription: must specify prod, dev, or test.
Conditions:
```