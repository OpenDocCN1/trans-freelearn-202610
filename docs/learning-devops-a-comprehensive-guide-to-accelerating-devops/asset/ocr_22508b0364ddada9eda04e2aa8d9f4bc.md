2. Now, let’s create a file named myinventory (without an extension) where we will write the IP addresses or the FQDN of the targets hosts, as shown in the following example:

```
192.10.14.10
mywebserver.entreprise.com
localhost
```

When Ansible is executed based on this inventory, it will execute all of the requested actions (playbook) on all the hosts mentioned in this inventory.

> Important Note
> For more information about the inventory file, read the documentation at https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html.

However, when you’re using Ansible in an enterprise, the same Ansible code (or playbook) contains the configuration actions that are performed for all of the VMs of an application. Since these VMs have different roles within the application, such as an application that consists of one (or more) web server and one database server, we must divide our inventory to separate the VMs by functional roles.

To group VMs by role in the inventory, we will organize our VMs into groups that will be noted between [], which gives us the following inventory:

```
[webserver]
192.10.20.31
mywebserver.exemple.com
[database]
192.20.34.20
```

In this example, we have defined two groups, webserver and database. All the hosts are distributed into these groups.

As another example, we can also group the hosts by environments with this sample inventory:

```
[dev]
192.10.20.31
192.10.20.32
[qa]
```