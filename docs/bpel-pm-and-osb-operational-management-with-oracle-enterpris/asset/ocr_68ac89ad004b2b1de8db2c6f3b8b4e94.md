# Challenges

For any small, medium, or large enterprise, the challenge is to manage several disparate application, middleware, and database targets from a single console. The traditional hardware and software components such as servers, storage devices, operating systems, databases, middleware, and applications have been managed by system management solutions. With the rise of integration software such as BPEL and OSB, applications and infrastructure pose new problems. First, they need to be managed along with existing hardware and software. Second, as the WebLogic domains and OSB projects proliferate across the enterprise, the infrastructure team is faced with increasing problems to manage these projects. Thirdly, for the administrators who are new to OSB, supporting these environments in production poses unknown challenges. Finally, the IT managers want to avoid hiring new administrators with increasing OSB projects.

# Solution

Grid Control provides a way to discover WebLogic and OSB components whether it is in development, test, stage, or production. For the infrastructure team, this is a convenient way to manage several installations from one console. The prerequisite is to have the Grid Control agent installed on the server hosting WebLogic admin server. Once the Grid Control agent is installed, the discovery process can be initiated from the console.

# Step-by-step exercises

This set of step-by-step exercises will walkthrough discovering a WebLogic and OSB target.

# Adding a WebLogic domain target

The first step in managing a WebLogic and OSB environment is to discover the WebLogic target – admin, clusters, and managed servers:

+   1. From the Grid Control home page, click on **Targets** | **Middleware**.
+   2. Click on the **Provisioning** sub-tab.