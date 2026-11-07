Your stack status should be in Create in Progress. Be patient, it may take a few minutes to complete. Once complete, you will see the status change to Create Complete. Take a look at Figure 9.8 for the finished product.

![](bbox=[0.02, 0.16, 0.92, 0.46])

Figure 9.8: Once completed, the stack status should show 'create complete'

We should now be able to see our newly created virtual machine and attached volume.

# CLI

Remember that in Chapter 6, we created the openrc-logistics file. We are going to use that file here. If you skipped Chapter 6, feel free to flip back to see how we created a new credential file that allowed us to scope to the logistics project.

Source the openrc-logistics file:

```
$ source openrc-logistics
```

Verify that the proper variables are set and we are scoped to logistics:

```
$ export | grep OS
```

Now that we are properly scoped, let's begin by listing any current stacks in the logistics project. We should have one stack from the previous Horizon dashboard exercise:

```
$ openstack stack list
```