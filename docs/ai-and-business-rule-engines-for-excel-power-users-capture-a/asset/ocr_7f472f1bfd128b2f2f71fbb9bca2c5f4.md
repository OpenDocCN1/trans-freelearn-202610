Deploying the KIE Server is similar to the four steps we list, except we deploy a *kie-server* image instead of *hello-world*. Now that we understand that we can deploy KIE Server to the cloud, it's a good time to remind ourselves of all the deployment options we have covered in this book.

# Roundup of decision model deployment options

Back in *Chapter 2*, we promised you four main ways to run our decision models. Now that we've completed deploying our rules into Docker-based servers, let's take a look at those options again. Our options for deploying our rules include the following:

+   - *Running KIE Sandbox with the web-based rules editor*: We have used this solution for most of this book, starting with *Chapter 3* and *Chapter 4*, as it is so easy to get started.
+   - *Running containers on your laptop or another computer (server) that you have access to*: We covered these methods in detail in this chapter and *Appendix C*, with containers giving us a wide range of deployment options.
+   - *Running an image in the cloud (hosted by Microsoft or IBM/Red Hat)*: We introduced this approach in *Chapter 5*, where we deployed our rules into OpenShift. We expanded this approach in *Chapter 9* and *Appendix B* to deploy into Codespaces and VS Code online on Azure. And this chapter introduced more cloud-based options using containers.
+   - We still have the final method to *set up the technology stack ourselves*: While we won't cover this, perhaps you have a bit more confidence to consider exploring this option than when you started this book. We suggest the KIE documentation as the next step.

# Summary

This chapter gave you another option to access the Scenario Simulation tool, this time using Business Central. Since our method for running Business Central relied on Docker, we took advantage of container technology to explore many different ways of deploying KIE Server images, from locally on our laptop through to scaling a deployment into the cloud. That gives us the confidence that we have many more options to edit and deploy our Business Rules within our organization as the use of decision models grows.

We will return to Business Central in *Chapter 12*, where we use it as a platform to demonstrate some advanced rule editing techniques. Docker will prove useful in the next chapter, as we use it to host Docker images for machine learning. And we'll return to Azure, using Machine Learning Studio as a tool to train our models.

This book set out to focus on rules-based decision models within AI. But we would be remiss if we didn't touch on the other main area of AI – machine learning. In the next chapter, we will learn how to combine the best of both AI approaches, using the tools that KIE gives us.