
Tasks that solely focus on some sort of batch processing can be run in isolation by leveraging the Docker-managed bridge network. For others, where you would like to expose the workloads outside the host, you could use the other two modes.

## Container secrets and kernel capabilities

Injecting secrets into the container environment can be achieved by defining the relevant keys, and respective values, in your container definitions. However, when dealing with secure parameters, you can make use of the Secrets Manager integration to avoid leaking any confidential data as part of the code, or related configurations.

All applications running inside containers need system call capabilities to perform some functions. ECS makes it possible to granularly assign the exact system calls that should be allowed by the particular task. Unless you are running container workloads that need to make some changes directly on the underlying host, you should try and avoid *privileged* mode. This essentially makes the container behave like a root user on the underlying host.

## Deploying a test application on ECS

It’s always fun to see what we’ve learned in action. Based on what we have explored so far, let’s get our hands dirty by deploying a test application on Amazon ECS. This time, we’ll create a new application that you can also use and customize beyond the scope of this chapter, as per your personal preferences – a **To-Do List Manager**.

We will sprinkle some data persistence elements into the application architecture to make it as close as possible to real-life usage patterns, where you would like to add new tasks and delete a few others daily. Let’s outline the main components of the application, as highlighted in *Figure 7.2*.

## Understanding the test application architecture

To-Do List Manager is an application that allows you to manage all the tasks that you accomplish in your day-to-day routine. In this exercise, we will develop and host two features: creating new tasks and deleting the ones that are complete. You can also further extend the scope of the application and personalize it to your taste. Several application- and infrastructure-level components communicate with each other to expose task management capabilities to the end user. They can be broadly categorized into two segments – **infrastructure components and application components**.

### Infrastructure components

These are the services that support the underlying infrastructure foundations on top of which the application runs. As always, you need a VPC, subnet, and corresponding route tables for traffic flow management. Once the user requests hit an Application Load Balancer hosted on the edge, it forwards the packets to an ECS cluster, which then returns an HTTP response, based on the type of request that was sent. As we’ve already discussed in this chapter, ECS is further composed of services, tasks, and task definitions. We will make use of all of them to host our Python-based web application in an AWS account.

As we saw in the previous chapter, A Programmatic Approach to IaC with AWS CDK, it’s very efficient to leverage existing CDK constructs to build and deploy AWS infrastructures at scale. This enables us to avoid investing a lot of time in writing CloudFormation templates from scratch. Therefore, we will define the entire infrastructure required in this chapter using AWS CDK.

The following are the three layers under the infrastructure segment:

- **Network infrastructure (VPCs, subnets, and route tables)**: We will create a standard 10.0.0.0/16 VPC CIDR block for this exercise and carve out two public and two private subnets, each with a /24 mask. We will offload all the tasks around route table creation, route entries, NAT gateways, and internet gateways to CDK using the aws-cdk-lib/aws-ec2 library.

- **Container platform (ECS)**: Within the scope of ECS, we will create task definitions that allocate compute and memory resources, and further define two containers for the application and the database. To have an always running application instance, we will wrap it into an ECS service that integrates with the Application Load Balancer. As we don’t want to manage any EC2 instances, we will use the Fargate launch type, thereby offloading this responsibility to the ECS service.

- **Traffic controller (Application Load Balancer)**: To receive requests from the end user, we will create a public-facing Application Load Balancer and drop its interfaces into the public subnets of the VPC. ECS automatically registers the running containers with the respective target group, thereby abstracting the container port, 5000 (used by Flask), behind the regular HTTP 80.

![](img/ecd265dfca9bb64161408ba8a944a4d9_176_0.png)

Figure 7.3 – Application components and the workflow of the To-Do List Manager application hosted on ECS

We intend to use CDK to deploy our infrastructure and application elements. So, let’s discuss the stack components we will require.

## Defining the CDK stack constructs

We will define the previously discussed infrastructure components inside a CDK stack. The complete CDK project is available in your Cloud9 IDE, in the chapter-7/chapter-7-cdk/ directory. We will not go through the CDK project initiation steps in detail as these were already covered in the previous chapter, A Programmatic Approach to IaC with AWS CDK. Let’s dive into the code-level constructs and gain some understanding of what is needed to get the application up and running. As usual, all the CDK construct definitions can be found inside the lib/ folder of the project directory – in this case, chapter-7/chapter-7-cdk/. Let’s dive into each infrastructure component in detail.

### Networking foundations and the ECS cluster

A common starting point for all infrastructure stack definitions is to create a VPC, subnets, and corresponding route tables. You will also need to set up an internet gateway for the VPC and a NAT gateway in each of the public subnets to enable packet traversals over the internet:

```
import { Vpc, SubnetType} from 'aws-cdk-lib/aws-ec2';
...
const customVPC = new Vpc(this, 'CustomVPC', {
  cidr: '10.0.0.0/16',
  maxAzs: 2,
  subnetConfiguration: [
    {
      subnetType: SubnetType.PRIVATE_WITH_EGRESS,
      name: 'privateSubnet',
      cidrMask: 24,
    },
    {
      subnetType: SubnetType.PUBLIC,
      name: 'publicSubnet',
      cidrMask: 24,
    }
  ],
  natGateways: 2
})
```

After adding the VPC resources, we are all set to define our ECS cluster and link it to this custom VPC:

```
import * as ecs from 'aws-cdk-lib/aws-ecs';
...
const ecsCluster = new ecs.Cluster(this, 'ECSCluster', {
  vpc: customVPC,
  clusterName: "aws-devops-simplified",
})
...
```

These simple constructs abstract a lot of complexity from the user. If you perform cdk synth at this point, you will appreciate the heavy lifting that goes on behind the scenes in generating the CloudFormation template for this stack.

Now that the cluster is ready, it’s time to define the task definition that will host both of our containers.

### Adding a task definition for our ECS cluster

As part of this configuration, we will also create the IAM policies that can be attached to the *task role* and the *execution role*. These permissions give these constructs access rights to the relevant AWS services:

```
import * as iam from 'aws-cdk-lib/aws-iam';
...
const execRolePolicy = new iam.PolicyStatement({
  effect: iam.Effect.ALLOW,
  resources: ['*'],
  actions: [
    "ecr:GetAuthorizationToken",
    "ecr:BatchCheckLayerAvailability",
    "ecr:GetDownloadUrlForLayer",
    "ecr:BatchGetImage",
    "logs:*",
    "elasticfilesystem:*"
  ]
});
const fargateTaskDefn = new ecs.FargateTaskDefinition(this,
"FargateTaskDefn", {
  cpu: 4096,
  memoryLimitMiB: 8192,
});
fargateTaskDefn.addToExecutionRolePolicy(execRolePolicy);
fargateTaskDefn.addToTaskRolePolicy(execRolePolicy);
```

You might notice that we have also added IAM permissions for the ECR service. This is to support the consumption of Docker images from the ECR repository, in case you decide to not use the existing image from DockerHub.

Task definitions help us configure the parameters, resources, and mount points that will eventually be used by containers provisioned as part of this task – the application and database containers in our case. Now, let’s proceed with defining the containers and their respective settings.

### Configuring the application and database containers

Think of all the configurations that would be needed for an application to run inside a container and respond to traffic originating from the internet. On the other hand, we don’t want to expose our database to the outside world and it’s just going to be available for the application over `localhost`. We will start by allocating a chunk of CPU and memory resources to both containers from the bigger *task* pool. By default, the Flask application runs on port 5000. This is something we want to expose to the outside world:
