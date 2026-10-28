# DevOps tooling

One of the primary themes of DevOps tooling is defining infrastructure as code. The idea is that you shouldn't manually perform a task, such as setting up a server, when you can create software to do it for you. You can then reuse these provisioning scripts, which will not only save you time but will also ensure that all of the machines are consistent and free of mistakes or missed steps.

# Provisioning

There are many systems available to commission and configure new machines. Some popular configuration-management automation tools are Ansible (https://www.ansible.com/), Chef (http://chef.io/), and Puppet (http://puppet.com/).

Not all of these tools work great on Windows servers, partly because Linux is easier to automate. However, you can run ASP.NET Core on Linux and still develop on Windows, using Visual Studio and testing in a VM or container. Developing for a VM is a great idea because it solves the problems in setting up environments and issues where it "works on my machine" but not in production.

![](bbox=[0.12, 0.53, 0.2, 0.61])

Vagrant (http://vagrantup.com/) is a great command-line tool to manage developer VMs. It allows you to easily create, spin up and share developer environments.

If you create your infrastructure as code, then your scripts can be versioned and tested, just like your application code. We'll stop before we get too far off topic, but the point is that if you have reliable environments, which you can easily verify, instantiate, and perform testing on, then CI is a lot easier.

# Monitoring

Monitoring is essential, especially for web applications, and there are many tools available to help with it. A popular open source infrastructure-monitoring system is Nagios (http://nagios.org/). Another more modern open source alerting and metrics tool is Prometheus (https://prometheus.io/).