The term we use for these virtualized physical machines is **virtual machine (VM)**. VMs are the virtualization of a computer’s physical memory and processor resources; physical computers are emulated in software. This allows a physical machine’s resources to be subdivided and have a portion allocated to each virtual machine. Each VM runs its own isolated OS, the same as a physical server. The OS accesses virtual compute resources such as **virtual processors (virtual CPU or vCPU)** and virtual memory.

The next section will look at the relationship between physical computers and VMs.

# Comparing virtual and physical machines

To understand the virtualization model over the physical model, we will briefly explore the physical hardware model of operation.

Historically, an organization operated on a model where, typically, the application and dedicated instance of an OS were mapped one-to-one to physical hardware. This was not very efficient in terms of costs or operations and also not very scalable since each app or new workload requested by the business required a new physical server. This led to scale-out server sprawl with increased rack space and associated costs.

This traditional physical approach model is outlined in the following diagram:

![](bbox=[0.225, 0.485, 0.775, 0.725])

Figure 9.1 – Physical server app deployment model

This traditional app delivery approach with a one-to-one relationship can benefit from the virtualization model. Virtualization addresses the following key challenges of the traditional physical hardware model:

+   - Utilization
+   - Scale
+   - Cost