We will see the options shown in the following figure:

![](bbox=[0.13, 0.12, 0.86, 0.3])

Figure 7.9 – Select service and compute tier

In this window, select the available tiers according to the needs of your workload. We can leave Service tier as General Purpose or we can select another option, such as Hyperscale or Business Critical, which are related to the vCore-based purchasing model, or Basic, Standard, or Premium for a DTU-based purchasing model.

In Compute tier, select Serverless to ensure the auto-scaling of the compute resources and that they are billed per second.

If we move to Compute Hardware, we can change the configuration of the hardware configuration. By default, it is Gen5, but we have more options. We will leave the default configuration.

Next, update the maximum and minimum number of vCores. By default, we have 2 for the maximum and 0.5 for the minimum, as presented in the following figure. We have more options to update, such as Auto-pause delay, which can be used to pause a database automatically if it is not active in a specific period.

![](bbox=[0.19, 0.54, 0.79, 0.64])

Figure 7.10 – Maximum and minimum vCores configuration for compute hardware

At the end, we have to choose Yes or No for database zone redundancy. After that, click on Apply.