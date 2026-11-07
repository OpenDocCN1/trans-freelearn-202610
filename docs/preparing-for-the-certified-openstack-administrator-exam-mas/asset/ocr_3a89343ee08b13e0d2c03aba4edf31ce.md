# CLI

Let's go ahead and learn how to add new groups and rules via python-openstackclient.

Source the openrc-logistics file:

```
$ source openrc-logistics
```

Verify that the proper variables are set to allow you to be scoped to the logistics project:

```
$ export | grep OS
```

Let's use python-openstackclient to list all security groups in our environment. You will notice we see all security groups, across all projects in the OpenStack environment (one default group within every project).

```
$ openstack security group list
```

![](bbox=[0.08, 0.55, 0.97, 0.68])

Figure 6.23: A list of all security groups in the OpenStack environment. There is one default security group per project

As we can see, the logistics project currently has two security groups: default and our newly created webserver-sg-dashboard.

Let's create a brand new security group:

```
$ openstack security group create webserver-sg-cli
```

And now we can easily add a new rule allowing inbound SSH (port 22) from anywhere.

```
$ openstack security group rule create --protocol tcp --ingress --dst-port 22 --src-ip 0.0.0.0/0 webserver-sg-cli
```