
## Task IAM Roles

In addition to defining a task execution role, you can also define roles for your containers running in the task. If your applications (containers) need to make API calls, such as accessing a database or storage service, a task IAM role can help you define the necessary permissions and grant temporary credentials as required. Task IAM roles are defined as part of the task definition or using the RunTask API operation. Task IAM roles also enable you to define different permissions for different tasks. This ensures the application of the principle of least privilege.

In addition to ensuring that your tasks have the correct permissions to access various AWS services, you must know how those tasks are deployed in your AWS account. Task placement strategies allow you to define how your tasks are deployed—for example, whether you want your tasks to run across multiple availability zones or in a single availability zone. In the next section, you will learn more about task placement strategies.

## Task Placements

When you launch your tasks using an EC2 launch type, you must define where those tasks run and which instances to use. Parameters for task placements are based on requirements such as CPU, memory, and port. You can also define strategies for terminating tasks when you want to scale down, allowing ECS to determine which tasks to terminate first.

In addition to placing tasks based on CPU, memory, and port, you can also define where the tasks are placed based on task placement constraints and define task placement strategies as follows.

### Task Placement Constraints

Task placement constraints are rules that govern how tasks are placed. They can be set when you create a new ECS service or update an existing one, or you can specify them for standalone tasks.

Constraints consist of a type, which defines the rule, and an expression, which specifies the instances attribute. Amazon ECS supports the following types of constraints:

- **distinctInstance**: Tasks are placed on different container instances.
- **memberOf**: Places tasks on container instances that satisfy an expression. You can specify the memberOf constraint when running a task, creating a new service, a new task definition, or a new revision of an existing task definition.

You can have custom or built-in attributes such as ecs.ami-id, ecs.os-type, and ecs.instance-type. For example, you can define a constraint to restrict the instance type to only t2.micro.

You can also group container instances using Cluster query language. For example, you can group container instances using attributes such as availability zone, instance type, or some other custom attribute. You can then customize how ECS places tasks based on this group.

Expressions have the following syntax: subject operator [argument]. As an example, the following expression uses instances in the eu-west-2a or eu-west-2b availability zone:

```
attribute:ecs.availability-zone in [eu-west-2a, eu-west-2b]
```

Another example is for task affinity, where you can select instances that host tasks in the service:development group:

```
task:group == service:development
```

Next, you can define task placement strategies.

### Task Placement Strategies

Amazon ECS supports the following placement strategies:

- **Binpack Strategy**: In a binpack strategy, tasks are placed on instances in a manner that leaves the minimum amount of unused CPU or memory. This placement strategy lets you minimize the number of container instances allocated to your tasks.

  An example of this strategy is as follows:

  ```
  "placementStrategy": [
    {
      "field": "memory",
      "type": "binpack"
    }
  ]
  ```

  Here, the strategy is binpack, and the placement of tasks is based on memory.

- **Random Strategy**: In a random strategy, tasks are placed at random. The following is an example of this strategy:

  ```
  "placementStrategy": [
    {
      "type": "random"
    }
  ]
  ```

- **Spread Strategy**: The spread strategy is where tasks are placed evenly according to a defined value. Acceptable values can be instanceId, host, or another custom attribute applied to the container instance, such as attribute:ecs:availability-zone, which enables you to spread the task across availability zones evenly.

  Here is an example of the spread strategy:

  ```
  "placementStrategy": [
      {
          "field": "attribute:ecs.availability-zone",
          "type": "spread"
      }
  ]
  ```

You can also define multiple task placement strategies using various strategies in the order you want them executed. For example, you can have a task placement strategy that distributes your tasks across availability zones evenly but then also binpacks those tasks based on memory. Here is an example:

```
"placementStrategy": [
    {
        "field": "attribute:ecs.availability-zone",
        "type": "spread"
    },
    {
        "field": "memory",
        "type": "binpack"
    }
]
```

Task placement strategies can be defined using the CLI as well as the console, as shown in Figure 11.4:

![Figure 11.4: Task placement strategies](img/34ff9b5b5a734dae3450ab701e223c73_455_0.png)

Note that these task placement strategies are only used when using the EC2 launch type, not Fargate. While task placement constraints and strategies enable you to configure how and where your tasks are placed, you must also consider your application's scalability requirements. Amazon ECS offers task layer and container layer scalability options, which will be discussed in the next section.

## Amazon ECS Scaling Options

Amazon ECS offers two different scaling options. The first is the service auto-scaling feature, which increases or decreases the number of tasks in your cluster. The second is the cluster scaling option, which handles the scaling out and scaling in of EC2 instances that host your tasks.

### Service Auto-Scaling

The ECS service auto-scaling automatically scales out and scales in the number of tasks that run in your cluster, and it uses the Application Auto Scaling service. This is applicable for both EC2 launch type and Fargate. Amazon ECS will publish CloudWatch metrics specifying average CPU and memory utilization, which are then used to determine when to scale out and scale back in.

Figure 11.5 depicts the auto-scaling process:

![Figure 11.5: Auto-scaling process](img/34ff9b5b5a734dae3450ab701e223c73_456_0.png)

The autoscaling workflow in the preceding diagram is as follows:

- The scaling policy is set to trigger when average CPU utilization reaches 70%.
- CloudWatch generates an alarm and notifies the Application Auto Scaling service.
- The Amazon ECS service launches another task to compensate.

The following scaling policies can be used with Amazon ECS:

- **Target tracking scaling policies**: Amazon ECS will launch or terminate tasks based on the pre-defined target value of a specific metric. For example, if you set the average CPU utilization target value to 50%, ECS will only launch that many tasks to ensure this target value is met. It will not launch additional tasks to bring the average CPU utilization below this value.

- **Step scaling**: Amazon ECS will launch or terminate tasks based on alarm breaches, and the size of the alarm breach is used to define how many tasks to launch.

- **Schedule scaling**: Amazon ECS will launch or terminate tasks based on the date and time you specify.

### Cluster Auto Scaling

Cluster auto scaling refers to scaling underlying EC2 instances to ensure sufficient resources are available for the tasks that run in a cluster.

To determine whether your instances need to be scaled out, ECS uses the **Auto Scaling group capacity provider** with managed scaling turned on. Amazon ECS creates two custom CloudWatch metrics – one for a low CloudWatch metric alarm and another for a high CloudWatch metric alarm. It then uses the **target tracking** scaling policy attached to the auto-scaling group. ECS then manages scale-in and scale-out actions based on the resource utilization of your tasks.

CapacityProviderReservation is a metric used to determine the remaining capacity in the auto-scaling group. If this metric is greater than targetCapacity, it indicates that more tasks are running and consuming a higher percentage of capacity. In response, the auto-scaling group launches additional EC2 instances registered to the cluster. Conversely, if this metric is less than targetCapacity, the auto-scaling group scales in.

To ensure that you only terminate those instances that don’t have any Amazon ECS tasks running, you must also turn on the **managed termination protection** feature.

Next, we will discuss why networking is critical when deploying applications on Amazon ECS. It dictates how your containers communicate with each other and the outside world. ECS offers several networking modes, each suited to different use cases.
