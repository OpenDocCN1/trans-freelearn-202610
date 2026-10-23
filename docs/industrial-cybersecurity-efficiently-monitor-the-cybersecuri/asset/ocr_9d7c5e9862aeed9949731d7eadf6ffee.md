# Exercise 3 – Setting up, configuring, and using Forescout's eyeInsight (formerly known as SilentDefense)

The final exercise for this chapter is about building and configuring a Forescout SilentDefense OT/ICS IDS and asset management appliance.

# Deploying the SilentDefense sensor and Command Center VMs

The Forescout SilentDefense sensor and Command Center appliances come as downloadable Open Virtual Appliance (OVA) template files and can be downloaded from the Forescout customer portal once you have purchased the product. OVA is a file format that supports the exchange of VMs across various virtualization products and platforms.

For this exercise, I will be using the following two OVA files:

+   - `sd-411-cc-16g-vmware.ova`: This is the 16 GB random-access memory (RAM) version of the Command Center.

+   - `sd-411-sensor-8g-vmware.ova`: This is the 8 GB RAM version of the sensor.

We will be installing the VMs on a vCenter cluster for two reasons, the first being that a vCenter cluster is typically the environment you would use for these types of appliances; secondly, I want to change things up a bit so that we get a better coverage of the tools, techniques, and technology typically found in the virtualization environment of the OT network. If you want to follow along with the exercise, VMware offers an evaluation version of vCenter that allows you to run the vCenter appliance for 60 days. Head on over to https://my.vmware.com/en/web/vmware/evalcenter?p=vsphere-eval-7 to find out more. Also, Chapter 19, Lab Setup, has instructions on how to deploy vCenter.

So, with the two OVA files downloaded and ready, follow along with these instructions to get our SilentDefense VMs deployed:

+   1. Navigate to the vCenter appliance portal (https://10.11.11.50) and click on LAUNCH VSPHERE CLIENT (HTML5), as illustrated in the following screenshot: