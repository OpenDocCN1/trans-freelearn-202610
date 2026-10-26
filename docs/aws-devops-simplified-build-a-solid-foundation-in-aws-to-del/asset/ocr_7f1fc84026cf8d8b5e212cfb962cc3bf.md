```
```javascript
...
const appContainer = new ecs.ContainerDefinition(this,
"AppContainerDef", {
  image: ecs.ContainerImage.fromRegistry("akskap/flask-todo-list-app"),
  taskDefinition: fargateTaskDefn,
  cpu: 512,
  memoryLimitMiB: 1024,
  logging: ecs.LogDrivers.awsLogs({streamPrefix: "aws-devops-simplified-app-container"})
})
appContainer.addPortMappings({
  containerPort: 5000
})
...
const dbContainer = new ecs.ContainerDefinition(this,
"DBContainerDefn", {
  image: ecs.ContainerImage.fromRegistry("mongo"),
  taskDefinition: fargateTaskDefn,
  cpu: 512,
  memoryLimitMiB: 1024,
  logging: ecs.LogDrivers.awsLogs({streamPrefix: "aws-devops-simplified-db-container"})
})

dbContainer.addMountPoints({
  containerPath: mountPath,
  sourceVolume: volumeName,
  readOnly: false,
})
...
```
```

As you can see, we are using separate image identifiers for both containers. One points to our Flask application code image, while the other points to an official MongoDB image, but both are hosted in DockerHub. You might also remember that we discussed mounting an EFS filesystem to the MongoDB container to persist all changes to the underlying data. This can be achieved with `dbContainer.addMountPoints`, as seen in the code. We also added a host port mapping for the application container using the `appContainer.addPortMappings` function.

At this point, however, ECS will complain that the mount point is not recognized. This is understandable because we haven’t provisioned an EFS filesystem yet, and secondly, we didn’t map it to the task definition before using it in the container definition. So, let’s ensure that these prerequisites are covered.