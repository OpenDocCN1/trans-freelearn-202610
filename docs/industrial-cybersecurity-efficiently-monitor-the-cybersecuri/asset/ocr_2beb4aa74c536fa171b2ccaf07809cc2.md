# Using Nessus

The following is the output from an advanced Nessus scan that's been performed on the two industrial switches (172.25.100.253, 254):

![](bbox=[0.14, 0.17, 0.86, 0.48])

Figure 17.14 – Nessus scan result for the IND switch

We can use this information to find exploits for the switches on online vulnerability and exploit databases.

There are many more tools that can find information about specific devices/brands; it helps tailor your pentest toolkit to the job at hand. For example, the next section will introduce a handy tool for getting information out of Cisco Networks called Cisco Torch.

# Using Cisco Torch

If we are dealing with Cisco environments, a tool that helps identify and characterize Cisco gear is cisco-torch. The tool doesn't come preinstalled with Kali Linux but can easily be added with the following command:

```
sudo apt install cisco-torch
```

Here is an example run of the tool:

```
cisco-torch -A 172.25.100.254
Using config file torch.conf...
Loading include and plugin ...
# Cisco Torch Mass Scanner #
```