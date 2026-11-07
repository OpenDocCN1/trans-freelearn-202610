# CLI

Source the newly created openrc-human-resources file. If you have not done this yet, refer back to the python-openstackclient section of the previous objective.

```
$ source openrc-human-resources
```

Verify that the proper variables are set:

```
$ export | grep OS
```

Now that we are properly scoped, let’s create a new virtual machine using the same parameters provided in the Horizon dashboard exercise.

```
$ openstack server create --image cirros --flavor m1.tiny --key-name my-new-keypair-cli \
instance3-cli
```

You should see similar output as Figure 5.12, while the instance is building.

![](bbox=[0.15, 0.39, 0.86, 0.82])

Figure 5.12: Output from the 'openstack server create' command