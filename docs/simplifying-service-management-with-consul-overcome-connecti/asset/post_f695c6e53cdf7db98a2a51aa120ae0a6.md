
+   4. Much like with the client configuration, we're configuring the address that we are going to bind to (`bind_addr`), the address that we are advertising (`advertise_addr`), and the address that we want to be identified as (`client_addr`). We're configuring these values using machine metadata that was set as part of the provisioning—another nice feature with Terraform and AWS.
+   5. Finally, we're enabling the web UI for Consul. By default, the web interface is not available, but we'll be using it occasionally throughout the book.

That completes the contents of the configuration file. The last thing we need to do is start our Consul servers!

Wahoo! Congratulate yourself for getting through all of that craziness! I hope it was as much fun for you reading it as it was for me writing it. If it makes you feel any better, *Planet Caravan* provides a great little beat for writing.

OK—by now, your Consul cluster should be up and running. Hey—*up and running*: that would be a great name for a book series! So, now that we've gotten it up, let's start taking a look around.

## Accessing the Consul system

The output from the Terraform run is going to provide you with vital information for accessing the Consul cluster. This is produced as *output* from the Terraform run. Another output you'll find in your directory is an SSH key that can be used for remotely connecting to the machines, should that ever be required, as it will be here shortly. Have a look at the following code snippet:

```
Apply complete! Resources: 25 added, 0 changed, 0 destroyed.

Outputs:

CONSUL_HTTP_ADDR = "http://18.191.150.155:8500"
Consul_Server_IPs = [
"3.129.42.175",
"18.191.150.155",
"3.139.81.1",
]
```

Using the preceding example, we're going to access the Consul system in two different ways: through the CLI and through the web interface.

## Using the CLI

Let's start with SSH. One of the artifacts produced from that Terraform run is an SSH PEM key used to authenticate yourself with the Consul machines. The file will be called `<owner>-consul-key.pem`. Using your command-line shell, from the same place you ran Terraform, you can SSH into any of the Consul servers—for example, this one:

```
```bash
$ ssh -i rjackson-consul-key.pem ubuntu@18.191.150.155
```
```

Obviously, you want to replace the name of the key with your own key and the IP address of one of the Consul servers. It can be either of the three, so don’t worry about picking the *right* one. You will receive a prompt with all sorts of gobbledygook (technical term) about the **Secure Hash Algorithm 256 (SHA256)** fingerprint and asking if you want to continue. Yes—you do want to continue, and you'll be on your very first Consul server!

> **Note**
> 
> If you don't want to SSH into the server, you don't need to! However, in order to communicate with the Consul cluster via the CLI, you will need Consul installed locally. Don't worry—it's as simple as downloading Packer and Terraform; download a ZIP file, unzip it, and place it in your path. Consul can be downloaded from https://www.consul.io/downloads. If you decide to utilize a remote CLI, just follow along, as the steps are the same.

Now that you're on the machine, please don't delete anything! Don't laugh—it happens. If you remember back when we were discussing the template file, there was a `bind_address` property that specified which address Consul was going to listen on. We are using the public address, so we want to set that as the Consul address in our environment. Don't worry—you have all of the information you need to *export* this information into your environment. I have no idea why they call it *export* when you are putting the information in, but so be it.

Within the Terraform output, you'll see a value for `CONSUL_HTTP_ADDR`. This is exactly the variable, along with that address, to set in your environment. Again using that Terraform output as an example, you can see this here:

```
```bash
$ export CONSUL_HTTP_ADDR=http://18.191.150.155:8500
```
```

OK—you’re all set, so let's take a look at that cluster! The first thing we should do is make sure that all of our machines, servers, and clients are up and registered in the Consul system. If you don't remember how many, we have three servers and four clients. You can see all of the *members* of our cluster via the following command:

```
```bash
$ consul members
```
```

Figure 2.19 – Consul web interface

Feel free to browse around here. You can already see a list of services that are available and where those services are running. Interestingly, we can see here that consul is running on three instances and httpd is running on four. Click on the consul row, and you can get information about the nodes running the service, as shown in the following screenshot. These are the Consul server nodes:

Figure 2.20 – Consul service instances

If we click on the **Nodes** tab at the top of the window, this will give us similar output to when we run our `consul members` command, as illustrated in the following screenshot:

![](img/52479dafaefba00517c48a137e037b7e_72_0.png)

Figure 2.21 – Consul nodes listing

And there you have it: you've created an entire Consul cluster, consisting of three servers and four clients. Congratulations!

## Keep It Safe, Stupid, and Secure Your Cluster!

At this point, you may be saying to yourself, *Wow, two chapters and I'm done.* Well, sorry to break it to you, but you are currently very much exposed. Yes, we've built a basic Consul cluster, consisting of three servers and four clients, but there is no security around that system. Essentially, we've built a glass house with no doors, so anybody can walk in, and anybody can see what's going on inside. Kind of creepy, isn't it?

To understand how we're going to protect our system, and more importantly our services, let's take a look at all of the areas where we are exposed:

- **System communication** – We have seen how all the Consul components communicate freely with each other. Anybody can join that cluster without verification, and anybody can listen in.
- **Who can do what** – Even if you are verified and authenticated within the cluster, we need to make sure that the scope of the actions available is tightly controlled. After all, we don't want just anybody jumping into the cluster with full access to the configuration.
- Extending Consul – With the understanding that multiple groups are going to access Consul, we want to give each of them the access they need to manage their own services, without interrupting others.

In addition to these areas, there are the basic procedures of securing the operating environment. This includes protected access to the operating system and the file structure within. These are considered basic administration procedures and aren't covered as part of this book.

With that, let's jump right in and start looking at how to start securing our glass house!

## Technical requirements

With the additional complexities introduced through securing any infrastructure, we'll be working with a new repository that is specific to this chapter:

```
https://github.com/PacktPublishing/Simplifying-Service-Management-with-Consul/tree/main/ch3
```

All the files and code are available, and we'll be utilizing Packer and Terraform again to create our working infrastructure, which means we'll also need the AWS credentials we established in Chapter 2, Architecture – How Does It Work?.

The exercises contained within this chapter have been performed within a macOS Terminal while utilizing the native SSH client. Familiarity with a command-line-based file editor is always helpful as well for viewing and editing files where necessary.

Finally, let's reiterate what was stated in Chapter 2, Architecture – How Does It Work?:

- Always perform a terraform destroy when you are done for the day. It does mean you'll have to rebuild as you continue the work the next day, but what a great lesson on immutable infrastructure!
- Be very careful with your AWS credentials. Never share your access key ID, and definitely don't share your secret access key. We're going to set them as environment variables so that they never show up in code, and absolutely never show up in your version control system!
