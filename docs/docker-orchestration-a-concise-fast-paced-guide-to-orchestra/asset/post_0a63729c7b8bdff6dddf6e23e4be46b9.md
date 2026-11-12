

"/packer/scripts/install-updates.sh",
"/packer/scripts/install-docker.sh"
  ]
}
]
}

First is the builders section. The only entry is the qemu builder which is specified by the type setting. The iso_* settings specify the URLs of the images to use, a checksum file to verify the images, and the type of the checksum. In this case, there is only a single image file. Since the image is a disk image rather than an ISO, the disk_image setting must be set to true.

The format setting sets the output format to qcow2. The format can also be set to raw. The disk_size setting sets the size of the new disk image in MB. This sets the size of the disk but does not change the file system. Cloud-Init will resize the root filesystem when the VM is booted with the disk image. The output directory and the name of the generated image are set in the output_directory and vm_name options, respectively. The path set in output_directory may be absolute or relative to the location where Packer is run.

By default, the qemu builder will start a VM using the SDL console. This will only work if X11 is available. In most cases, the headless setting should be set to true. The console is available via VNC to watch the console during the provisioning process.

The ssh_* settings control how the qemu builder connects to the VM for provisioning. The ssh_username and ssh_password settings set the credentials needed to log in to the VM. In this case, it is configured to use the root user and the password set when the image was prepared. The qemu builder will continually try to connect to SSH after the VM starts, but will give up after the amount of time specified in ssh_wait_timeout. Ensure that the timeout provides enough time for the VM to boot.

The provisioners section is a list of provisioners that are to be used to provision the image. This example has a single shell provisioner, which runs the scripts specified in the scripts section on the VM. The install-updates.sh script simply runs apt-get update && apt-get dist-upgrade -y. The second script, install-docker.sh, is the install script from get.docker.com.

When the configuration is ready, the image can be built by running `packer build <config>`, where `<config>` is the name of the configuration file. A sample is given here. Most of the output has been removed for brevity:

```
$ packer build xenial-cloudimg.json
qemu output will be in this color.
```

```
==> qemu: Downloading or copying ISO
qemu: Downloading or copying: file:///packer/images/xenial-server-cloudimg-amd64-prep.img
==> qemu: Copying hard drive...
==> qemu: Resizing hard drive...
==> qemu: Found port for communicator (SSH, WinRM, etc): 3475.
==> qemu: Looking for available port between 5900 and 6000 on 127.0.0.1
==> qemu: Starting VM, booting disk image
qemu: The VM will be run headless, without a GUI. If
you want to
qemu: view the screen of the VM, connect via VNC without a password to
qemu: 127.0.0.1:5983
==> qemu: Waiting 10s for boot...
==> qemu: Connecting to VM via VNC
==> qemu: Typing the boot command over VNC...
==> qemu: Waiting for SSH to become available...
==> qemu: Connected to SSH!
==> qemu: Provisioning with shell script: /packer/scripts/install-updates.sh
==> qemu: Provisioning with shell script: /packer/scripts/install-docker.sh
==> qemu: Gracefully halting virtual machine...
==> qemu: Converting hard drive...
Build 'qemu' finished.

==> Builds finished. The artifacts of successful builds are:
--> qemu: VM files in directory: /packer/images/xenial-cloudimg
```

![](img/02bead9c8ff4515f78dac0eb33163961_257_0.png)

Ensure the user running Packer has permission to use KVM, when using the `qemu` builder. Qemu may throw permission-denied errors if the user does not have the access needed. On Ubuntu, the user needs to be in the `kvm` group.

The run will take a few minutes and, assuming everything runs properly, the new image will be placed in the directory set in the `output_directory`. Packer will not run if the output directory exists unless the `-force` option is set to avoid accidental deletion of build images.

## Using Puppet with Packer

Provisioning a new image with scripts works if the needed changes are relatively simple. More complicated changes require more complicated scripts. Configuration management tools are ideally suited to the task and can be used to provision the new image. Ansible, Chef, Puppet, and Salt are all supported.

An added benefit to using a configuration management tool to provision an image is that the same configuration can be used to ensure that it stays properly configured while the server is running. The exact process to do that will vary depending on the tool being used.

The following Packer snippet uses Puppet in the masterless mode to configure the image. The same Puppet manifest that was used in the Using configuration management section is used here as well. A command to join the Docker cluster should not be included. The command to join the cluster can be run after a new instance is started in the cluster:

```
"provisioners" : [
{
	"type" : "shell",
	"scripts" : [
		"/packer/scripts/install-updates.sh",
		"/packer/scripts/install-puppet.sh"
	]
},
{
	"type" : "puppet-masterless",
	"manifest_file" : "/packer/puppet/docker-host.pp"
}
]
```

The first difference is that the install-docker.sh script was replaced by install-puppet.sh which runs apt-get install -y puppet. Second is the new provisioner block with a type of puppet-masterless. The manifest_file option specifies the manifest that will be used to configure the image; a directory may also be used. In that case, all of the manifests will be applied to the node.

## Treating infrastructure like code

Let's pause for a moment and consider what was just done. The server has been completely defined in one or more files with a scriptable build process. We have just turned our servers into code.

The server images can be built through a CI process. Rather than performing updates on the running servers, new images can be built that incorporate the updates. The images can be fully tested through a test suite before being deployed. They can be fully vetted in a test environment before being deployed into production. Even the deployment process can be controlled through the CI process, just like the Docker images.

The other benefit of this approach is that it is very easy to roll back updates. If a problem is discovered, new servers can be started on the previous, working image to restore services quickly. The broken image can be tested and debugged offline or in a different environment.

This is, basically, the same approach to running versioned containers. The important thing from a troubleshooting perspective is that important data, such as logs, are not stored on the server. Instead, they should be stored on network storage or shipped to services such as Elasticsearch and Kibana.

## Post-build configuration

Using a custom image does not eliminate the need for boot-time and ongoing configuration management. Boot-time configuration can be minimized to simple things like the hostname or joining a cluster. When running in multiregion or multicloud environments, Cloud-Init sets information specific to the region.

Configuration management of a running system is still important. It can ensure that the configurations that were put in place when the server was built, remain in place. It can also make simple changes such as updating administrators' SSH keys without needing to fully rebuild the image.

## Automating host deployment

There are many ways to automate the deployment of Docker hosts. The specifics, as always, depend on the orchestration system. For example, adding new hosts to a Docker Swarm cluster running on Amazon EC2 is as easy as running something like the following:

```
$ docker-machine create --driver amazonec2
$ docker swarm join --token ABC... manager
```

For Azure or GCE, it would be the same. Simply replace the driver and pass the appropriate driver specific flags to docker-machine.

