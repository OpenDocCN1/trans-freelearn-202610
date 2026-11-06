You should also be aware that security wasn't a primary concern of this chapter's examples, so don't consider these exhaustive examples ready to be applied in a production environment, but instead as possible starting points for common aspects of nearly every Elixir application:

+   - Configuring a Distillery release with custom release tasks
+   - Containerizing an application with the end goal of building a streamlined release image and simplifying development in the local environment
+   - Interacting with a Kubernetes cluster by using `kubectl` and by applying Kubernetes configuration files
+   - Using a continuous integration tool, such as Travis CI, to automate the deployment process
+   - Connecting Erlang nodes running in different Kubernetes pods

Security in a cloud environment, building releases, containerization, Kubernetes, and continuous deployments are all complex topics, each deserving its own book. This chapter serves as an introduction to many of these concepts, and shows how a single person armed with a laptop can serve their application to the world in a robust and scalable way, by relying on the amazing cloud tools that exist today.

In the next chapter, we will learn how to collect application metrics and easily check, profile, and trace running processes, expanding our visibility about what's happening with our application in the production environment.