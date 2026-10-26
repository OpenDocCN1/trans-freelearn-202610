
```yaml
ImageId: ami-0834874e33262e665
InstanceType: t3.micro
SubnetId: subnet-0e4f97bea5c50b873
SecurityGroupIds:
  - sg-06320d12095603e68
KeyName: test-key-pair
```

Deploying this template will create an EC2 instance with all the configurations defined under the `TestEC2Instance:` section. You will notice that most of the configurations reference pre-existing resource identifiers. This means that they should be present in the respective AWS account (and region) before you deploy a stack using this template.

> **Tip**
> 
> Most of the AWS services are region scoped and you should take special care about the implications this could have. For example, a public AMI available in `eu-central-1` cannot be referenced in your CloudFormation template when you’re deploying a stack in another region. Furthermore, AWS ensures that none of your resources transfer data to another region unless they’ve been explicitly configured by the user. You should keep this in mind when you’re working on architectures that need to comply with strict data residency and compliance needs.

Therefore, please keep in mind that hard-coding configurations is not a good design practice as stack deployments will fail if prerequisites don’t exist. Secondly, it’s difficult to reuse such templates in other AWS accounts and regions. Security, reusability, and modularity should be at the top of your mind when you’re working with them. We will cover some good design practices later in the *Best practices for using CloudFormation to define enterprise-grade architectures* section.

To ensure reusability and a clean design, CloudFormation offers the capability to use **parameters**, which can be defined at runtime. This enables dynamic configuration possibilities and simplifies template maintainability.

## Parameters

These are the values that you pass to your template during stack creation. It helps separate the design from the configuration. You can have defaults, and also provide a list of allowed values, to control what the user can or cannot use. Once the parameters have been defined, it is easy to reference them in the Resources section using the Ref keyword:

```yaml
Parameters:
  InstanceType:
    Type: String
    Default: t3.micro
    AllowedValues:
      - t3.micro
```

```yaml
  - m5.medium
...
...
Resources:
  EC2Instance:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType:
        Ref: InstanceType
...
...
```

This makes your templates flexible as they can adapt to the values provided at runtime.

## Conditions

**Conditions** provide a control mechanism in your template. Based on a certain condition being true or `false`, you could either skip the resource creation altogether or use a different configuration value when deploying the stack. This is useful in situations where you would like different values for development or production, for example:

```yaml
...
Conditions:
  SkipResourceCreation: !Equals
    - !Ref Environment
    - dev
...
...
Resources:
  EC2Instance:
    Type: AWS::EC2::Instance
    Condition: SkipResourceCreation
...
```

But what if you would like to block the stack’s operation based on the evaluation of certain parameter values? Let’s have a look at **rules**.

## Rules

This is an optional section of your template that can be used to evaluate a certain combination of parameter values and block the stack creation or update process. It works around the concepts of a *rule condition* and an *assertion*. If you don’t define any condition, then the rule assertion will always be evaluated:

```yaml
Rules:
  testInstanceType:
```

```yaml
RuleCondition:
  !Equals - !Ref Environment
  - test
Assertions:
  - Assert:
      'Fn::Contains':
        - - t3.medium
          - !Ref InstanceType
      AssertDescription: 'For a test environment, the instance type must be t3.medium'
```

This rule denies stack updates or creation when the instance type is different than `t3.medium`.

So far, we’ve covered different ways to pass information to a CloudFormation template. What if we need some information in return after stack operations are complete? This is where **outputs** come into the picture.

## Outputs

These are the values that you would like to retrieve after the stack operations have been completed. You can access them via the console or CLI commands, or reference them in other templates as *imports*. This approach ensures that the templates are of a manageable size as you don’t have to put all the resources together in one big file. You can define an output value using the Outputs construct:

```yaml
Outputs:
  InstanceID:
    Description: ID of the EC2 instance
    Value: !Ref EC2Instance
```

As soon as the stack operations are complete, you can access the identifier of the new EC2 instance using the `InstanceID` key.

The concepts we’ve covered so far should help you get started with the practical day-to-day usage of the CloudFormation service. To make debugging issues easier, let’s discuss the internals of how CloudFormation works under the hood.

## How CloudFormation works

AWS services such as CloudFormation are great at reducing the cognitive load of the end user. A lot of internal details are abstracted, exposing only the functionalities that the users need. However, in certain situations, it’s important to understand what the service is internally doing. This not only helps debug problems faster but also gives you a good thinking ground when discussing topics such as security, compliance, and day-to-day operations for your organization.

multiple API calls in the background, all targeted toward realizing the infrastructure stack described in your CloudFormation template.

AWS CloudTrail’s event history describes all the event payloads from the APIs triggered by the service on your behalf. Let’s see what events are logged during the creation of a simple stack:

```
AWSTemplateFormatVersion: 2010-09-09
Description: Demonstrate CloudTrail log integration
Resources:
  MyS3Bucket:
    Type: 'AWS::S3::Bucket'
    Properties:
      BucketName: devops-simplified-cfn-test-bucket
  MyEC2Instance:
    Type: 'AWS::EC2::Instance'
    Properties:
      ImageId: ami-0c956e207f9d113d5
      InstanceType: t3.micro
      SubnetId: subnet-0a09e573bc8792a95
```

Let’s deploy our stack in the eu-central-1 region using the AWS CLI:

```
aws cloudformation deploy \
  --stack-name devops-simplified-test \
  --template-body file://template.yml \
  --region eu-central-1
```

Once the stack has been created, you can see the logged API calls in the CloudTrail console. These are highlighted in Figure 4.2:

![](img/ecd265dfca9bb64161408ba8a944a4d9_88_0.png)

Figure 4.2: API calls logged in CloudTrail as a result of stack creation

## Best practices for using CloudFormation to define enterprise-grade architectures

When it comes to architectural designs spawning multiple accounts, teams, and regions, there are a few guiding principles you should always keep in mind.

### Keep templates small and reusable

When you are just getting started, you work with small templates targeting a minimal set of resources. Over time, the needs of your team and organization grow, which can lead to an uncontrolled expansion of the same template for all future additions. It is a good practice to restrict the scope of these templates based on the team’s responsibilities and the applications they own. Managing infrastructure resources for two different applications in the same stack can have adverse effects on the team’s agility and introduce dependencies. Furthermore, it also introduces an element of risk as some stack operations can affect all other resources.

Secondly, maintaining small templates promotes reusability. For example, a three-tier web application following all the best practices and organizational security controls could easily be shared with other teams to get them started with a secure and compliant infrastructure baseline.

Managing dependencies between different stacks can be simplified with inputs and outputs.

### Leverage inputs and outputs for cross-stack dependencies

As discussed previously, CloudFormation can export certain resource identifiers for use by other stacks. This can be achieved by defining Output parameters and exporting these values with a predefined name. In such cases, the service acts as a message bus facilitating communication between different stacks.

Stacks that import these values use the Fn::ImportValue intrinsic function:

```
Resources:
  WebServerInstance:
    Type: 'AWS::EC2::Instance'
    Properties:
      InstanceType: t3.micro
      ImageId: ami-0c956e207f9d113d5
      NetworkInterfaces:
        - GroupSet:
          - Fn::ImportValue
            'Fn::Sub': '${NetworkStack}-SecurityGroupID'
```

In this example, the stack is importing SecurityGroupID from another stack whose name is referenced by the ${NetworkStack} parameter.
