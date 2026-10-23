If everything is correct, you can click the Create button, and the container will be deployed. When you navigate to the cargo-container instance overview, you can view information about the container, as shown in the following screenshot:

![](bbox=[0.112, 0.145, 0.852, 0.375])

Figure 9.15 – Overview of the cargo-container instance running in the Azure cloud

As you can see from the CPU and Memory graphs, the container is up and running. Let’s see if it works by going to the home page of the Cargo Tracker application that is running inside this container. To visit this container, we need to copy the FQDN from the overview that you can find on the right side. This FDQN is not enough; you also need to add this to the end of the FQDN: :8080/cargo-tracker/. We need to add this because the container is listening on port 8080 and the application itself listens to the cargo-tracker path. The complete URL will look something like this: http://cargo-tracker.g8gah7eecrmbxcy.westeurope.azurecontainer.io:8080/cargo-tracker/. When you navigate to this URL, you will see the home page of the Cargo Tracker application, meaning that the container and application are running in the cloud:

![](bbox=[0.27, 0.57, 0.695, 0.795])

Figure 9.16 – Home page of the Cargo Tracker application