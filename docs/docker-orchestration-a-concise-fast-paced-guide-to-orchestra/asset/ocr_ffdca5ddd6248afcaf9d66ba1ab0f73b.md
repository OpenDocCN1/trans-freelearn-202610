The command is passed to /bin/sh -c "". Any double quotes in the command must be escaped. To complicate matters, the escape character (\) must also be escaped. A command that includes a double quoted component looks something like this:

```
"healthChecks" : [
  {
    "protocol" : "COMMAND",
    "command" : {
      "value" : "your_check_health_command.sh "Foo""
    }
  }
]
```

# Deploying rolling updates

Existing applications are updated with dcos marathon update. Previously, this command was used to scale the number of instances of an application. It can also be used to update an application to use a new image:

```
{
  "id" : "nginx",
  "instances" : 1,
  "cpus" : 0.25,
  "mem" : 64.0,
  "container" : {
    "type" : "DOCKER",
    "docker" : {
      "image" : "nginx:1.10",
      "network" : "HOST"
    }
  }
}
```

This is a simple Ngnix application.

Note that the image tag is set to nginx:1.10. It just so happens that 1.11 has been released and now it is time to update the application. There are two ways to do it.