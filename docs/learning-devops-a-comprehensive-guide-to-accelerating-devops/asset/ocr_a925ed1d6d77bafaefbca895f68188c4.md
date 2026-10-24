# # 5 Authoring the Development Environment with Vagrant

In the previous chapters, we learned how to provision a piece of infrastructure with Terraform, how to install middleware with Ansible, and, finally, how to create Virtual Machine (VM) images with Packer.

One problem that is frequently pointed out by operational teams is the fact that they need to be able to test all of the automation scripts on isolated environments, that is, environments that are not on the local machine. Indeed, writing scripts with Ansible under Linux, for example, is difficult to test on a local Windows machine.

To answer this problem, we can use virtualization systems such as Hyper-V or VirtualBox, which allow you to have VMs with different operating systems that run locally. However, to go even further, we can automate the creation and provisioning of these VMs using the tiers tool from HashiCorp, called Vagrant.