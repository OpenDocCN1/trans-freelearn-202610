# # Using Ansible in a Packer template

We have just seen how to write a Packer template that uses command scripts (for example,
apt-get), but it is also possible to use Ansible playbooks to create an image. Indeed,
when we use IaC to configure VMs, we are often used to configuring the VMs directly
using Ansible before thinking about making them into VM images.

What is interesting about Packer is that we can reuse the same playbook scripts that we
used to configure VMs to create our VM images. So it's a huge time-saver because we
don't have to rewrite the scripts.

To put this into practice, we will write the following:

+   - An Ansible playbook that installs NGINX

+   - A Packer template that uses Ansible with our playbook

Let's start with the writing of the Ansible playbook.

# # Writing the Ansible playbook

The playbook we are going to write is almost identical to the one we set up in Chapter 3,
Using Ansible for Configuring IaaS Infrastructure, but with some changes.

The following code is the sample of the playbook:

```
```
---
- hosts: 127.0.0.1
become: true
connection: local
tasks:
- name: installing Nginx latest version
apt:
name: nginx
state: latest
- name: starting Nginx service
service:
name: nginx
state: started
```
```