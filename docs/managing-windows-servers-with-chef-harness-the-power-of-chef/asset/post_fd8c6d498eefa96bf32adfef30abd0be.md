

Create the role from the JSON file with the following `knife` command:

`knife role from file umbraco.json`

As you can see, the preceding JSON file defines a role (as specified by "chef_type": "role") which declares that the role's `run_list` contains one entry, the umbraco recipe, as shown in the following code snippet:

"run_list": [
    "recipe[umbraco]"
],

# Applying the role to the node

In order to apply the role to our newly bootstrapped node, we will need to get a list of the hosts and then edit its configuration and apply the new role to the host's configuration as follows:

1. First, get a list of nodes with `knife node list` as follows (in our case, we have a list of one host, the newly created host):

   `knife node list`

   `WIN-CJDQ9DEOJFK`

2. Edit the node configuration for the host you want to install Umbraco on with `knife node edit HOSTNAME`, for example:

   `knife node edit WIN-CJDQ9DEOJFK`

This will open the node's configuration JSON in your editor as follows (in our case, the default configuration for the host that was just created):

```
{
    "name": "WIN-CJDQ9DEOJFK",
    "chef_environment": "_default",
    "normal": {
        "tags": [
        ]
    },
    "run_list": [
    ]
}
```

Notice that the `run_list` property is currently empty (this may look a bit different if you are using an existing host and not a newly provisioned one). Add the `umbraco` role to the node's JSON under the `run_list` property as follows:

```
"run_list": [
	"role[umbraco]"
]
```

3. Apply the role to the desired nodes with `knife` using a simple search for all hosts with the `umbraco` role as follows:

   ```
   knife winrm 'role:umbraco' 'chef-client' -x Administrator
   ```

4. You will see the output of the Chef client pass by on the screen. Once it has finished, and assuming that everything went well, you can visit the newly installed Umbraco CMS by browsing to the `/install` URL at the default HTTP service on your host (in this example scenario, it would be `http://WIN-CJDQ9DEOJFK/install`). You will be guided through the steps required to finalize your Umbraco installation such as selecting the database, creating an administrative user, and selecting any starter kit and themes.

## Summary

At this point, you should have a fully functioning installation of Umbraco on your Windows server without having to manually configure anything. As you can see, you could now repeat this across one, tens, or hundreds of hosts if needed by simply bootstrapping and configuring the roles for those hosts.

In the next chapter, let's take a look at how to automate the provisioning of hosts when they need custom configuration during the bootstrapping phase, and how we can use Chef to manage cloud hosts.

# 5 Managing Cloud Services with Chef

One of the very powerful uses of Chef is leveraging it to extend your infrastructure into the cloud as seamlessly as onsite hardware. Chef has a number of mechanisms for integrating with popular cloud service providers. In this chapter, we find out how to use Chef to manage and deploy our software to a variety of popular cloud service providers.

In this chapter, we will cover the following topics:

- Microsoft Azure
- AWS EC2
- Rackspace Cloud

## Working with Microsoft Azure

Azure is Microsoft’s competitor to EC2 and Rackspace Cloud. Any of the three will provide you with Windows virtualization, so included in this chapter is the information on how to integrate Azure with Chef.

The `knife-azure` gem provides the functionality needed to control your Azure account via the `knife` utility. Chef uses plugins to provide the extended functionality, including managing cloud services. Support for Azure is present in a Ruby gem named `knife-azure` and is installed via the `gem` command-line utility as follows:

```
gem install knife-azure
```

This command will install all the gems that `knife-azure` is dependent upon,
not just Chef alone.

> As of this writing, the `knife-azure` gem has a dependency on an
> older version of bundler, which can cause issues and may require
> that you manually downgrade the version of bundler that is installed.
> Using RVM or another Ruby manager will help isolate these issues and
> allow you to manage your gems.

## Downloading the management certificate

In order to use the Azure API, you need to have a management certificate
that identifies you with the service. You can create and download a generated
management certificate from `https://manage.windowsazure.com/`
`publishsettings/index?client=xplat`.

## Configuring knife for Azure

Now that you have your management certificate inside your `.publishsettings` file,
`knife` needs to know about it. This is done, as you might expect, through the `knife.`
`rb` configuration file. To let `knife` know where to find your newly downloaded
`.publishsettings` file, first move it to somewhere (such as your home directory
or `$HOME/.chef`) and then add the following line to your `knife.rb` file:

`knife[:azure_publish_settings_file] = "/path/to/publishsettings_file"`

Now that `knife` has all the information it needs, we will be able to make API calls
to Azure.

## Creating a new Azure virtual machine

In order to create a new server, you need to know what image to use; you can view
a list of available images using the `image list` subcommand of the `azure` plugin
as follows:

`knife azure image list`

Windows-Server-2012-Essentials Windows East Asia, Southeast Asia
Ubuntu-12_04_2-LTS-amd64-server Linux East Asia, Southeast Asia
Windows-Server-2008-R2 Windows West US, East Asia

As you can see, the output will list the image identifiers (shortened for print), the
platform that they provide, and the regions that they are available in. The list of
available images will be quite long and changes over time as Azure adds more base
images to its growing list; thus, this command will come in handy later on any time
you provision a new virtual machine. This list provides you with two pieces of
information that you will need in order to construct a new host: the image identifier
followed by the regions that the image is available in.

Using this information, let’s take a look at the following commands to see how we
could provision a new Windows Server 2012 Datacenter edition host with 127 GB
of storage in the Western US datacenter with a DNS name of `00c0ff3300`:

```
knife azure server create --azure-dns-name "00c0ff3300" \
--azure-service-location "West US" \
--azure-source-image a699494373c04fc0bc8f2bb1389d6106__Windows-
Server-2012-Datacenter-201401.01-en.us-127GB.vhd \
--winrm-user 'azure' --winrm-password '+0ps3kr3+' \
--distro 'windows-chef-client-msi'
```

After issuing this command, you will see the status of the host provisioning as it
progresses. Chef will wait until the virtual machine has been provisioned and is
marked as ready. During this phase, you can see the status and how long it takes
for Azure to reach that state using the following command:

```
Waiting for virtual machine to reach status 'provisioning'..... vm state
'provisioning' reached after 2.67 minutes
```

Once the host is in the ready state, Chef will be able to tell you a little more about
the host, including the IP addresses and WinRM port. You should expect an output
similar to the following command:

```
DNS Name: 0c0ff330.cloudapp.net
VM Name: 0c0ff330
Size: Small
Azure Source Image: a699494373c04fc0bc8f2bb1389d6106__Windows-Server-
2012-Datacenter-201401.01-en.us-127GB.vhd
Azure Service Location: West US
Public Ip Address: 138.91.227.224
Private Ip Address: 100.82.76.57
WinRM Port: 5985
Environment: _default
```

The provisioning process may take a while, depending on the location and base image that you selected. During the provisioning process, it will create the WinRM user specified on the command line, azure in this case, with the specified password. However, WinRM’s basic authentication will not be turned on by default; authentication via Kerberos will be enabled by default, but Kerberos support is outside the scope of this book. As a result, you will encounter errors here about being unable to bootstrap the host, which we will fix by enabling WinRM’s basic HTTP authentication mechanism in the next set of steps.

## Bootstrapping your Azure node

Your initial **virtual machine** (VM) will need to have basic authentication enabled for WinRM; once this is done, you can save your newly created image as a reusable Azure image with your administrative credentials and WinRM basic authentication enabled.

To turn on WinRM support for basic authentication, you will need to perform the following steps:

1. Enable RDP on your Azure instance.
2. Use Remote Desktop to connect to your new virtual machine and log in with the credentials specified during the provisioning phase (here, the account was `azure` and the password was `+0ps3kr3+`).
3. Allow WinRM to accept basic HTTP authentication by opening the Windows command prompt and running the following command:

   ```
   winrm set winrm/config/client/auth @{Basic="true"}
   ```

At this point, you can return to your workstation and bootstrap your new host with the `knife bootstrap` command as follows:

```
knife bootstrap windows winrm 00c0ff3300.cloudapp.net \
  --winrm-user azure \
  --winrm-transport ssl
```

The preceding command will prompt you for your administrator password. After entering the correct credentials, you can proceed to bootstrap the Chef client on your new Azure host and register it with your Chef server. Once it has completed, you can verify that it was properly registered with the `node list` command as follows:

```
$ knife node list
00c0ff3300
i-49117415
```

Notice that among our registered nodes, our new virtual machine is now displayed.
