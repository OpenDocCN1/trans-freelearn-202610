We can now log in to our instance:

username: cirros

password: cubswin:)

Now that we are logged in, let's attempt to ping (3 times) openstack.org. Be patient! It may take a few moments to see the ping replies.

```
$ ping -c 3 openstack.org
```

You should see the following response:

![](bbox=[0.08, 0.3, 0.96, 0.72])

Figure 6.56: A ping response from openstack.org confirms successful north/south traffic communication

If you perform a ping without the -c count argument, you can cancel it by pressing press Ctrl + C.