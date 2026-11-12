# ## Introducing environments

Rancher supports running multiple environments. After installation, a single environment named `default` will be created that runs the Cattle orchestration suite. Additional environments can be created as needed. The environments are isolated from each other. This makes it easy to create environments for development, testing, production, or environments for different business units.

While Cattle is the default orchestration tool, Rancher also has the ability to create new environments running Docker Swarm, Kubernetes, and Mesosphere. This is probably the easiest way to install either of those systems in a local environment. Once the new environment is created, add a new host. The services for the environment, such as `kubectl` or the Swarm agent, will be added automatically.

For more information on using Docker Swarm refer to Chapter 4, Orchestration with Docker Swarm, for Kubernetes refer to Chapter 5, Deploying and Managing Services with Kubernetes, and for Mesosphere refer to Chapter 6, Working with Mesosphere.

# ## Managing services

Cattle uses Docker Compose files to define services. As of the 1.1.4 release, only version 1 of the Docker Compose specification is supported. Version 2 support is being added in Rancher 1.2.

Rancher Compose builds on the standard `docker-compose.yml` file in two ways. First, special labels can be added to services to define things such as affinity. Second, another file called `rancher-compose.yml` can be used to define Rancher-specific settings such as health checks.

> > The following sections discuss using the Rancher CLI. All of the following actions can also be done through the web interface.

# ## Starting a stack

By design, the `rancher-compose` tool is very similar to `docker-compose`. A **stack** is a collection of services defined in a `docker-compose.yml` file and an optional `rancher-compose.yml` file. There are pre-built stacks for many applications in the Rancher catalog. In addition to quickly deploying services, they can be great examples for building your own stacks.