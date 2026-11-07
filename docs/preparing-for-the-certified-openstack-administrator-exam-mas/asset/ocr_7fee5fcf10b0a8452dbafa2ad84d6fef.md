Congratulations! You have just successfully allowed a total of 20 virtual machine instances to be booted within the production project. Keep in mind that this is the total number of instances within the production project and not per user.

# CLI

To get a comprehensive list of default quotas for a specific project, use python-openstackclient. First ensure that you have sourced your openrc file:

```
$ source openrc
```

Verify that the variables have been set:

```
$ export | grep OS
```

Get a list of all projects in the environment:

```
$ openstack project list
```

Find the Trade project UUID. The UUID in your environment may be different than what appears here. Use the openstack quota show command to see all the default quotas for the trade project.

```
$ openstack quota show eb1a2580ea87490bbe206162ecf2fde5
```