After you have created provider-network-dashboard, you should see it in the list of networks in the Networks panel in the Admin-System panel group. See Figure 6.44. Notice how the provider network shows Yes in the External column.

![](bbox=[0.07, 0.18, 0.95, 0.38])

Figure 6.45: A newly created provider network on the Horizon dashboard

# CLI

It's time to jump on the command line again. This time, we will be creating a provider network. Remember: we want the provider network to exist inside the admin project so users in other projects can't modify it. In that case, we only need to source the openrc file.

```
$ source openrc
```

Verify that the proper variables are set and you are scoped to the admin project:

```
$ export | grep OS
```

Now let's create our new provider network. This time, we will specify the public-cli for the provider's physical network. This is a physical network mapping that I previously provisioned for the virtual appliance. Like the public-dashboard physical network we used in the previous exercise, this provides your OpenStack environment with access to your system's internet connection. Also take note of the --external flag. Without this flag, your users will not be able to set their router's gateway to the provider network.

```
$ openstack network create --provider-physical-network public-cli --provider-network-type flat provider-network-cli --external
```