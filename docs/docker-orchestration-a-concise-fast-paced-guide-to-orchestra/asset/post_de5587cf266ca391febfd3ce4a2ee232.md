
When the manifest is ready, it can be applied to the server with `puppet apply` if running masterless, or with puppet agent when using Puppet Server:

```
$ puppet apply docker-host.pp
Notice: Compiled catalog for ubuntu in environment production in 0.43 seconds
Notice: /Stage[main]/Main/Node[default]/Exec[install docker]/returns:
executed successfully
Notice: Finished catalog run in 31.86 seconds
```

Puppet Server provides for easy distribution of manifests to servers in the cluster. Documentation on configuring Puppet Server may be found at https://docs.puppet.com/puppetserver/latest/services_master_puppetserver.html.

All that this configuration does is install Docker. Adding it to the cluster is a different matter. A check should be written to see if the host is part of the cluster, and if not, run the appropriate commands to join the cluster. These scripts will vary depending on the orchestration tool used.

The `exec` resource in the following snippet runs `docker swarm join`, and if successful, touches the `/root/swarm-joined` file:

```
exec { 'join swarm' :
  command => "/usr/bin/docker swarm join --token SWMTKN-... manager:2377
&& /usr/bin/touch /root/swarm-joined",
  creates => '/root/swarm-joined'
}
```

Every time `puppet apply` is run, it will check to see if `/root/swarm-joined` exists. If it does not, Puppet will rerun the command to join the swarm. This is an overly simplistic check since it does nothing to attempt to re-establish a connection to the swarm. However, if the node had joined successfully before, it will attempt to rejoin on its own.

## Configuring a server on boot

Using configuration management to configure a host is great, but there is a problem with it. There was the manual step of installing Puppet and running `puppet apply` the first time. The fewer manual steps there are in an automated environment, the lesser chance there is of something getting missed or done differently.

There is a solution which works very well for servers running in a cloud environment called **Cloud-Init**. Cloud-Init was created specifically to deal with the problems of configuring servers started in AWS and similar services. The base image was fine to start with, but things such as user accounts and applications needed to be added or run when the server was started and people did not want to have to log in to every machine by hand to add them.

Cloud-Init has made several appearances in this book already. *Chapter 1, Getting Started with Docker Orchestration* showed how to use it to add login credentials to CoreOS, RancherOS, and Project Atomic hosts. It showed up again in *Chapter 3, Cluster Building Blocks – Registry, Overlay Networks, and Shared Storage* and *Chapter 7, Using Simpler Orchestration Tools – Fleet and Cattle* when enabling Flannel and Fleet on CoreOS. The cloud images for Ubuntu, RHEL, Fedora, and CentOS all have support for Cloud-Init.

The configuration file for Cloud-Init is a YAML file that starts with `#cloud-config` as the first line. It is usually added to a new server instance through a field named `user-data`. It can also be made available through a mounted disk such as a CD-ROM. Full documentation is available at https://cloudinit.readthedocs.io/en/latest/index.html.

Let's first assume that the image being started needs to have Docker installed and will be joining a Docker Swarm. The following example shows how to do both of these things. It first downloads and runs the Docker install script. Then Cloud-Init will run the command to join a Docker Swarm:

```
#cloud-config
runcmd:
- "/usr/bin/wget -O - http://get.docker.com | /bin/bash",
- ['/usr/bin/docker', 'swarm', 'join', '--token', 'SWMTKN-...',
'manager:2377']
```

The runcmd module takes a list of commands to run when the instance is first booted. Each command can be either a string or a list.

Cloud-Init can also set up and run Puppet. The following example will install Puppet and run it on boot:

```
puppet:
install: true
conf:
server: "puppet.example.com"
certname: "%i.%f"
```

The conf section contains key/value pairs that will be added to the [puppetd] section of /etc/puppet/puppet.conf on the client. The server option is the address of the Puppet server. The certname option sets the name of the certificate that the client will use to authenticate. The %i and %f variables are the instance ID and the fully qualified domain name, respectively.

## Using a custom image

The problem with installing and configuring an application with configuration management or Cloud-Init is that it can lead to subtle differences in the versions of the installed software. It also takes extra time when the server first boots to install and configure everything. A faster and more consistent solution is to use an image which has been custom built for the task. CoreOS and Project Atomic are two distributions that are custom built to run Docker containers and very little else. It is also possible to build a custom image with all of the required software pre-installed.

## Building a new image with Packer

HashiCorp has created a tool called Packer, which can be used to create custom images for use with OpenStack, AWS, Azure, GCE, and VMWare. It can also create images for Xen, Linux KVM, and can even deploy to hardware via cobbler.

A Packer configuration is a JSON file. There are three major sections, which are as follows:

- The builders section is a list that contains the list of image types to build.
- The provisioners section is a list of tasks to perform to provision an image. This includes running scripts, uploading files, and running configuration management.
- Finally, the post-processors section contains a list of tasks to do after the build is completed.

Those tasks can generate a manifest, run local commands, or upload an image to VMWare vSphere or Amazon S3.

Multiple images can be built from the same configuration file. Each image will have an entry in the `builders` list. This allows multiple images to be built in parallel using the same `provisioners`. For example, this could be used to build a new image to be deployed to Amazon, Azure, and locally to OpenStack.

> Full documentation for Packer is available at https://www.packer.io/docs/.

### Preparing the Ubuntu Cloud Image

This example uses the `qemu` builder for easy testing on a local Linux box. The `qemu` builder works from a base image. By default, the new image is installed from an ISO installer, but it can also be cloned from an existing disk image. This example uses the Ubuntu Cloud Image. This is the same basic image that is available on most cloud providers. The image is available at https://cloud-images.ubuntu.com.

The cloud image is a great starting point because it is a very minimal server installation. It has two problems that need to be fixed before it can be used with Packer. First, the `qemu` builder uses SSH to log in to the system for provisioning. Unfortunately, there are no accounts available to log in with. Second, the `qemu` builder can only log in with a username and password, but the SSH daemon is configured to deny password authentication. The following script fixes both of these problems. It must be run as root to allow it to mount the image and make the necessary changes:

```bash
#!/bin/bash

IMAGES=/packer/images
BASE=xenial-server-cloudimg-amd64-disk1.img
NEW=xenial-server-cloudimg-amd64-prep.img
MNT=/mnt

cd $IMAGES
cp $BASE $NEW

modprobe nbd max_part=16
qemu-nbd -c /dev/nbd0 $NEW
partprobe /dev/nbd0
mount /dev/nbd0p1 $MNT
sed -i 's/^PermitRootLogin .*$/PermitRootLogin yes/'
$MNT/etc/ssh/sshd_config
sed -i 's/^PasswordAuthentication no/PasswordAuthentication yes/'
$MNT/etc/ssh/sshd_config
chroot $MNT sh -c 'echo "root:s3cret-s@uce" | /usr/sbin/chpasswd'
umount $MNT
qemu-nbd -d /dev/nbd0
md5sum $NEW > $NEW.md5sum
```

The script copies the original image to a new file and mounts the new file. It then reconfigures sshd to allow password login in general and root logins specifically. Next, it uses chpasswd within chroot to set a root password. Finally, it unmounts the image and creates a checksum file.

Instead of using the root user, a separate build user could be added to the image. To simplify the build process, the image should be configured to allow the build user to use sudo without a password. The user could then be removed at the end of the build process.

## Building the image

Now that the image is prepared, it can be used to build a new image. Following is a Packer configuration that will build a new image that will install all available updates and install Docker:

```
{
"builders" : [
{
"type" : "qemu",
"iso_urls" : ["file:///packer/images/xenial-server-cloudimg-amd64-prep.img"
],
"iso_checksum_url" : "file:///packer/images/xenial-server-cloudimg-amd64-prep.img.md5sum",
"iso_checksum_type" : "md5",
"disk_image" : true,
"output_directory" : "/packer/images/xenial-cloudimg",
"vm_name" : "xenial-cloudimg-amd64-custom.img",
"shutdown_command" : "shutdown -P now",
"format" : "qcow2",
"disk_size" : 20000,
"headless" : true,
"ssh_username" : "root",
"ssh_password": "s3cret-s@uce",
"ssh_wait_timeout": "10m"
}
],
"provisioners" : [
{
"type" : "shell",
"scripts" : [
```
