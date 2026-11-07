
See Figure 6.43. Provider networks will only show up in the Admin-System panel group section of the Horizon dashboard.

![](img/da864e0670c65edf6f5ab861ef9906b3_216_0.png)

Figure 6.43: Creating a provider network from the Horizon dashboard

- 1. From the OpenStack overview page, select the **Admin** panel group.
- 2. Select the Networks Panel on the **Admin-System** panel group. Here you should see all current tenant networks owned by all projects in the environment. You can identify them as tenant networks because you can see that the **External** column is set to No.
- 3. Let’s create a brand new provider network. Select **Create Network**.

See Figure 6.44. The **Create Network** screen for provider networks is a bit different than the one for tenant networks previously reviewed in this chapter. Let’s explore the options:

- 1. **Network Name** (optional): The name for your tenant network. Enter `provider-network-dashboard`.
- 2. **Project** (required): The specific project we want to own this provider network. In our environment, regular users will not be modifying the provider network, so let’s place it in the **admin** project.
- 3. **Provider Network Type** (required): We can specify a tagging or encapsulation method for the provider network. This is useful if we want the provider network to link to a resource that resides on a specific VLAN, VXLAN, or GRE tunnel. We also have the ability to select FLAT to represent no encapsulation or tagging of the provider network traffic. Let's specify FLAT.
- 4. **Physical Network** (required): The name of the physical interface mapping we want associated with this provider network. This is not an interface name (such as eth0, eth1, or eth2) but rather a label defined in the file `/etc/neutron/plugins/ml2/linuxbridge_agent.ini`. I have previously created a mapping between the interface veth1 on the virtual appliance and the label `public-dashboard`. This network will provide your OpenStack environment access to the outside world, a.k.a. the internet (as long as you have internet access on your host machine). Type in `public-dashboard`.
- 5. **Segmentation ID** (optional): You only need to specify the segmentation ID if you specify a VLAN, VXLAN, or GRE Provider Network Type in Step 6. This could be a VLAN tag, VXLAN VNI, or GRE ID. We will leave this blank since we are using Provider Network Type FLAT (no encapsulation/tagging).
- 6. **Admin State** (required): The Admin State controls whether the provider network is off or on. If you set it in the DOWN state, any subnets you associate with the provider network will not be functional until turned back to UP. Verify it is set to UP.
- 7. **Shared** (optional): If a provider network is shared, all projects across all domains will be able to directly attach their instances to this provider network. We are only going to allow our users to set their router gateways to the provider network, so we will make sure we uncheck this option.
- 8. **External Network** (optional): This is the magic that allows our users to set their router's gateway to the network. This will allow us to get our virtual machine instances access to the resources that live outside OpenStack. Let's make sure it is checked.

- 9. Select **Submit** to create the network.

## Create Network

### Description:

Create a new network for any project as you need.

Provider specified network can be created. You can specify a physical network type (like Flat, VLAN, GRE, and VXLAN) and its segmentation_id or physical network name for a new virtual network.

In addition, you can create an external network or a shared network by checking the corresponding checkbox.

| Field | Value |
|---|---|
| Name | provider-network-dashboard |
| Project * | admin |
| Provider Network Type * | Flat |
| Physical Network * | public-dashboard |
| Segmentation ID * |  |
| Admin State * | UP |
| Shared |  |
| External Network |  |
| Cancel |  |
| Submit |  |

![](img/da864e0670c65edf6f5ab861ef9906b3_218_0.png)

Figure 6.44: Entering provider network details on the Horizon dashboard

After you have created provider-network-dashboard, you should see it in the list of networks in the Networks panel in the Admin-System panel group. See Figure 6.44. Notice how the provider network shows Yes in the External column.

![](img/da864e0670c65edf6f5ab861ef9906b3_219_0.png)

Figure 6.45: A newly created provider network on the Horizon dashboard

## CLI

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

You should see the following output:

![](img/da864e0670c65edf6f5ab861ef9906b3_220_0.png)

Figure 6.46: Output from the 'openstack network create' command

Confirm provider-network-cli was successfully created by running the following:

```
$ openstack network list --long
```

Congratulations! You've created a brand new provider network on the CLI.

## Exam objective - manage provider subnets

Managing provider subnets is identical to managing tenant subnets. We will follow a very similar process to the tenant subnet creation process outlined earlier in the beginning of this chapter. One slight difference is that our provider subnet will not have DHCP enabled. Remember: our users will not be attaching instances directly to the provider network and subnet. When we demonstrate north/south traffic in the next section, you will only set your router gateway to the provider network/subnet.

It's also important to note that the subnet CIDR defined in the provider subnet will become the available floating IP address pool. We will learn about floating IP addresses in the last section of this chapter.

## Horizon dashboard

- 1. See Figure 6.47. Select `provider-network-dashboard` from the Networks panel under the Admin-System panel group.
- 2. See Figure 6.48. Select the **Subnets** tab.
- 3. Select **Create Subnet**.
- 4. See Figure 6.49. **Subnet Name**: `provider-subnet-dashboard`

![](img/da864e0670c65edf6f5ab861ef9906b3_221_0.png)

Figure 6.47: Creating a provider subnet on the Horizon dashboard

![](img/da864e0670c65edf6f5ab861ef9906b3_221_1.png)

Figure 6.48: Creating a provider subnet on the Horizon dashboard

![](img/da864e0670c65edf6f5ab861ef9906b3_222_0.png)

Figure 6.49: Filling in provider subnet details on the Horizon dashboard

- 5. Network Address: 172.16.0.0/24. Because you most likely only have access to one public IP address in your home, we will temporarily suspend our disbelief and select an internal IP address range to act as our external provider network IP address. We are using this private network range because the virtual appliance has a previously created gateway interface, which will allow the provider network to talk out to the internet connection on the host machine. In the real world, this could be an external IP address range provided by an Internet Service Provider.
- 6. IP Version: IPv4
- 7. Gateway IP: This is the previously created gateway interface on your virtual appliance. This will typically be the IP of your gateway device or router that lives outside OpenStack. For the sake of this exercise, we will enter 172.16.0.1. In your virtual appliance, this is an IP address assigned to a virtual interface that routes out to your VirtualBox NAT network and out the default gateway on your host system. You can see it by running `ip a show tap0` inside the virtual appliance.
