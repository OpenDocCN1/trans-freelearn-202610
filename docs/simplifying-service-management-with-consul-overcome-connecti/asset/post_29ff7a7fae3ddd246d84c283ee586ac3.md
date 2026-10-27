
Have a look at the following diagram:

![](img/52479dafaefba00517c48a137e037b7e_49_0.png)

Figure 2.16 – Gossip connections

All of the members of the Consul cluster, the servers and clients, are swimming in the same pool. This enables the distributed discovery of all members of the pool, their status, and—of course—the status of any services registered with the Consul clients. Although gossip is how all of the agents learn about changes to cluster members as well as services, all information is still sent from the agents to the server nodes via direct RPCs. Remember, any server within the peer set can receive an update and forward this to the leader for quorum commitment and an update to the state machine.

## Changes in latitudes

We've already seen how Consul shares information within a cluster regarding leadership election, changes in service availability, and changes in node availability. In addition to these items, Consul is always aware of the geographic location of each node relative to others within the cluster. This can help with intelligent decisions such as locating the nearest available service instance to the requesting entity. The discovery mechanism utilizes network tomography offered by the Serf library. Yup—not only do they all gossip about each other's state, but they also know each other's location! The singularity is upon us!

## Setting up our first Consul cluster

The hour arrived! At the beginning of this chapter, there were instructions for creating credentials for an AWS account, as well as for downloading Terraform. Now, you finally get to put all of that work to good use!

> **Note**
> 
> This book covers aspects of Consul for both server and client operations. HashiCorp also offers a managed Consul service on **HashiCorp Cloud Platform (HCP)**, which may be desirable when deploying within your own production environment. However, in order to understand the server operation, we'll be operating on our own. For more information about HashiCorp’s hosted Consul service, visit `https://portal.cloud.hashicorp.com/`.

All of the code for this section is in the following GitHub repository:

https://github.com/PacktPublishing/Simplifying-Service-Management-with-Consul

You can either fork the repository or just download it and play along. We're going to be building our system for each chapter with code in that repository, with the relevant code in the appropriate chapter folder.

The following sections include a review of the Packer and Terraform code that we'll be using to create our Consul cluster. It isn't necessary to understand every aspect of the code, but I wanted to include explanations for completeness. As you proceed, however, take careful note of the commands indicated by the `$` symbol.

## Building the image

We're going to start by building our base image. In order to keep us all on the same page—quite literally—regarding the image being used and the operation of Consul, we're going to be building an image together using Packer. But don't worry—all of the code you need is in the repository. Within the ch2 folder, you'll find another folder called Image-Creation. I know—very inventive. Here's the code you'll need to get to this folder:

```
```sh
$ cd $github_root/Consul-Up-and-Running/ch2/Image-Creation
```
```

There are a few key items in here to be aware of. Your variables for the build are all defined in `variables.pkr.hcl`, as illustrated in the following code snippet:

```
```hcl
variable "owner" {
  type        = string
```
```

```
description = "Owner tag to which the artifacts belong"
default     = "packt-consul"
}
variable "consul_version" {
    type = string
    description = "Three digit Consul version to work with"
    default = "1.9.0"
}
variable "aws_region" {
    type        = string
    description = "AWS Region for image"
    default     = "us-east-2"
}
variable "aws_instance_type" {
    type        = string
    description = "Instance Type for Image"
    default     = "t2.small"
}
```

For most of these, the defaults will be fine. However, if you want to change any of the defaults, make that change in variables.pkrvars.hcl. For example, look at the following snippet:

```
owner = "rjackson"
```

Yup—customizing your variables is just that easy! The Packer file that defines what we are building is called AWS_linux_image.pkr.hcl. Let’s take a quick look at that file, just to see what we are building.

Lines 1 through 19 define the source image that we’re going to be using to build our Consul image. We have variables set for the name and region, utilizing the aforementioned variable files. We’re also filtering through AWS to find the appropriate Ubuntu image, as illustrated in the following code snippet:

# AWS_linux_image.pkr.hcl

```
source "amazon-ebs" "ubuntu-image" {
    ami_name = "${var.owner}-consul-{{timestamp}}"
    region   = "${var.aws_region}"
```

```
instance_type = var.aws_instance_type
tags = {
    Name = "${var.owner}-consul"
}
source_ami_filter {
    filters = {
        virtualization-type = "hvm"
        name = "ubuntu/images/*ubuntu-bionic-18.04-amd64-server-*"
        root-device-type = "ebs"
    }
    owners = ["099720109477"]
    most_recent = true
}
communicator = "ssh"
ssh_username = "ubuntu"
}
```

Lines 21-58 actually build the image, including all the provisioning that we need. Essentially, we are taking that initial image we searched for and packing what we need into it—hence the name Packer. As part of this process, we are moving a couple of files over for Consul to utilize: one service file defining how Consul is to be managed within the Ubuntu environment, as well as a common configuration file that will be applied to all Consul nodes. We're also performing some inline provisioning, instructing Packer to ssh to the nodes and execute a set of commands as part of the setup. The code is illustrated in the following snippet:

```
build {
    sources = [
        "source.amazon-ebs.ubuntu-image"
    ]
    provisioner "file" {
        source      = "../files/consul.service"
        destination = "/tmp/consul.service"
    }
    provisioner "file" {
        source      = "../files/consul-common.hcl"
        destination = "/tmp/consul-common.hcl"
    }
```

```
```hcl
}
provisioner "shell" {
  inline = [
    "sleep 30",
    "sudo apt-get update",
    "sudo apt install unzip -y",
    "sudo apt install default-jre -y",
    "curl -k -O \"https://releases.hashicorp.com/consul/${var.consul_version}/consul_${var.consul_version}_linux_amd64.zip\"",
    "unzip consul_${var.consul_version}_linux_amd64.zip",
    "sudo mv consul /usr/local/bin"
  ]
}
provisioner "shell"{
  inline = [
    "sudo /usr/local/bin/consul -autocomplete-install",
    "sudo useradd --system --home /etc/consul/consul.d --shell /bin/false consul",
    "sudo mkdir /etc/consul /etc/consul/consul.d /etc/consul/logs /var/lib/consul/ /var/run/consul/",
    "sudo chown -R consul:consul /etc/consul /var/lib/consul/ /var/run/consul/",
    "sudo chmod -R a+r /etc/consul/logs/",
    "sudo mv /tmp/consul.service /etc/systemd/system/consul.service",
    "sudo mv /tmp/consul-common.hcl /etc/consul/consul.d/consul-common.hcl"
  ]
}
}
```

One of those configuration files includes the common settings that we want across all Consul nodes. For our purposes, we're only going to set this to the log settings. We're going to set these to DEBUG so that we can poke around and learn more about what Consul is doing! Have a look at the following code snippet:

# consul-common.hcl

```
log_file = "/etc/consul/logs/"
log_level = "DEBUG"
```

At this point, you might want to double-check to make sure your AWS environment variables are set. You can do this with the following command:

```
$ env |grep AWS
AWS_ACCESS_KEY_ID=AFAIKJENNY
AWS_SECRET_ACCESS_KEY=8675309...
```

Now, we're ready to build! Execute the simple Packer command shown in the following code snippet and take a few minutes to congratulate yourself for packing an image! Celebrate with a beverage of choice and relax, as the process will take about 5 minutes:

```
$ packer build -var-file="variables.pkrvars.hcl" .
```

Reviewing the output, it will look like somebody threw Christmas lights all over your terminal. The Packer output is all red and in various shades of green, just to make things festive. You'll see files getting moved over, as well as the operating system updating, just to make sure we've got all of the latest packages for Ubuntu. You'll also see some lines about provisioning with a shell script. Packer creates shell scripts based on those shell provisioning blocks we saw within the Packer build definition.

At the end of the process, you'll see the location and the Amazon Machine Image (AMI) ID for your new image! This is illustrated in the following snippet. Now, you can build upon your Consul knowledge in your own image:

```
==> Wait completed after 5 minutes 52 seconds
==> Builds finished. The artifacts of successful builds are:
--> amazon-ebs.ubuntu-image: AMIs were created:
us-east-2: ami-079a52bd81f91b201
```

Again, there is no need to remember that image ID, as in our next step, Terraform will find it for us!
