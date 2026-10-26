I would like to highlight the use of two intrinsic functions that were referenced in this template:

+   - Importing stack outputs from a different deployment:
+     Fn::ImportValue, as its name suggests, is used to import the outputs from another stack. We used the aws cloudformation list-exports command to list the exports from the network stack. The same values are referenced here, using this function. This is how CloudFormation allows you to resolve inter-stack dependencies while keeping the templates modular, and teams agile.
+   - Substituting data for generating configurations at runtime:
+     Not all configurations can be statically defined in the templates. As seen in this example, the network stack could have been named in different ways. To keep the template definition flexible, we substituted the value of the respective parameter to form a well-defined string. Eventually, this allows us to import the required value from another stack.

# ALB listeners and target groups

To make the ALB understand the type of traffic it is handling, we can use the concept of listeners. They identify the protocol, port, and actions to take when packets are received. Listeners also map to a target group, which is where the matching traffic needs to be forwarded:

```
Parameters:
...
  NetworkStackName:
    Description: Identifies the name of the network stack.
    Type: String
...
Resources:
...
  AppLoadBalancerListener:
  Type: AWS::ElasticLoadBalancingV2::Listener
  DependsOn: AppLoadBalancer
  Properties:
    Protocol: HTTP
    Port: 80
    LoadBalancerArn: !Ref AppLoadBalancer
    DefaultActions:
      - Type: forward
        TargetGroupArn: !Ref InstanceTargetGroup

  InstanceTargetGroup:
  Type: AWS::ElasticLoadBalancingV2::TargetGroup
  Properties:
    HealthCheckEnabled: true
```