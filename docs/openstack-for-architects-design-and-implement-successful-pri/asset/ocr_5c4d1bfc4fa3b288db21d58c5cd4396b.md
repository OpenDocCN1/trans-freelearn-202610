![](bbox=[0.17, 0.12, 0.81, 0.52])

# Implementing H/A in the lab deployment

In this last section, we'll make the current lab deployment that we deployed in Chapter 2, Architecting the Cloud, highly available by adding a second controller node and configuring cluster software. This example should be simple enough to easily implement in the lab while still allowing you to evaluate the technology.

# Provisioning a second controller

In later chapters we'll look at more flexible deployment methodologies that allow fine-grained service placement and automated cluster configuration. In this chapter, we'll just extend the Packstack deployment from Chapter 2, Architecting the Cloud. Packstack isn't designed to deploy multiple controllers, so some manual configuration of services will be required.