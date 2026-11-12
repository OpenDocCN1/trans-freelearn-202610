

```
name="dm-azure-test-firewall"
(dm-azure-test) Attempting to clean up Availability Set resource...
name="docker-machine"
(dm-azure-test) Removing Availability Set resource... name="docker-machine"
(dm-azure-test) Attempting to clean up Subnet resource... name="docker-machine"
(dm-azure-test) Removing Subnet resource... name="docker-machine"
(dm-azure-test) Attempting to clean up Virtual Network resource...
name="docker-machine-vnet"
(dm-azure-test) Removing Virtual Network resource... name="docker-machine-vnet"
Successfully removed dm-azure-test
```

> There are many options for the Azure driver including options to choose the image, VM size, location, and even which ports need to be open on the host. For full documentation refer to https://docs.docker.com/machine/drivers/azure/.

## Installing on a running Linux host

You can also use a generic driver of docker-machine to install and manage Docker on an existing host running a supported Linux distribution. There are a couple of things to keep in mind. First, the host must already be running. Docker can be pre-installed. This can be useful if you are installing Docker as part of your host build process. Second, if Docker is running, it will be restarted. This means that any running containers will be stopped. Third, you need to have an existing SSH key pair.

The following command will use SSH to connect to the server specified by the --generic-ip-address flag using the key identified by --generic-ssh-key and the user set with --generic-ssh-user. There are two important things to keep in mind for the SSH user. First, the user must be able to use sudo without a password prompt. Second, the public key must be in the authorized_keys file in the user's $HOME/.ssh/ directory:

```
$ docker-machine create --driver generic --generic-ip-address 52.40.113.7 --generic-ssh-key ~/.ssh/id_rsa --generic-ssh-user ubuntu dm-ubuntu-test
```

This process will take a couple of minutes. It will be faster than the creates on cloud services that also have to provision the VM. Once it is complete, you can manage the host with docker-machine and start running containers.

The only difference between the generic driver and the other cloud drivers is that the `stop` subcommand does not work. This means that stopping a generic Docker host has to be done from the host.

Full documentation can be found at
https://docs.docker.com/machine/drivers/generic/.

## Introducing Docker-specific distributions

One of the benefits of running services with Docker is that the server distribution no longer matters. If your application needs CentOS tools, it can run in a container based on CentOS. The same is true for Ubuntu. In fact, services running in containers based on different distributions can run side-by-side without issue. This has led to a push for very thin, Docker-specific distributions.

These distributions have one purpose: to run Docker containers. As such, they are very small and very limited in what comes out of the box. This is a huge benefit to cloud wranglers everywhere. Fewer tools mean fewer updates and more uptime. It also means that the host OS has a much smaller attack surface, giving you greater security.

Their focus on Docker is a great strength, but it can also be a weakness. You may find yourself up against a wall if you need something specific on your host that is not available. On the positive side, many tools that might not be available in the default install can be run from a container.

## CoreOS

CoreOS (https://coreos.com) was one of the first Docker-specific distributions. They have since started their own container project called **rkt**, but still include Docker. It is supported on all major cloud providers including Amazon EC2, Microsoft Azure, and GCE and can be installed locally in bare-metal or in a local cloud environment.

CoreOS uses the same system that Google uses on their Chromebooks to manage updates. If the updates cause a problem, they can be easily rolled back to the previous version. This can help you maintain stable and reliable services.

CoreOS is designed to update the system automatically which is very unique. The idea is that automatically updating the OS is the best way to maintain the security of the infrastructure. This process can be configured, by default, to ensure that only one host in a CoreOS cluster is rebooting at a time. It can also be configured to only update during maintenance windows or turned off completely. Before you decide to turn it off manually, remember that a properly configured orchestration system will keep services up and running even when the hosts they are running is on reboot.

CoreOS includes Docker but does not enable it. The following example from the CoreOS documentation shows how to enable Docker on boot. This is done by creating a new systemd unit file through cloud-init. On AWS, this is placed in the user data instance configuration:

```
#cloud-config

coreos:
  units:
    - name: docker-tcp.socket
      command: start
      enable: true
      content: |
        [Unit]
        Description=Docker Socket for the API

        [Socket]
        ListenStream=2375
        BindIPv6Only=both
        Service=docker.service

        [Install]
        WantedBy=sockets.target
```

CoreOS uses a default core user. Users can be added through the cloud-config file:

```
#cloud-config
users:
  - name: "demo"
    passwd: "$6$HpqJOCs8XahT$mSgRYAn..."
    groups:
    - "sudo"
    - "docker"
```

An SSH key can also be added with the `ssh-authorized-keys` option in the `users` block. You can add any number of keys to each user:

```
#cloud-config
users:
- default
- name: "demo"
  ssh-authorized-keys:
- "ssh-rsa AAAAB3Nz..."
```

CoreOS also supports `sssd` for authentication against LDAP and **Active Directory (AD)**. Like Docker, it is enabled through `cloud-config`:

```
#cloud-config
coreos:
units:
- name: "sssd.service"
command: "start"
enable: true
```

The `sssd` configuration is in `/etc/sssd/sssd.conf`. Like the rest of CoreOS, the configuration can be added to `cloud-config`:

```
#cloud-config
write_files:
- path: "/etc/sssd/sssd.conf"
permissions: "0644"
owner: "root"
content: |
config_file_version = 2
...
```

> Full configuration of `sssd` is beyond the scope of this book. Full documentation is at the following website:
> https://jhrozek.fedorapeople.org/sssd/1.13.1/man/sssd.conf.5.html

## RancherOS

Rancher (http://rancher.com) was designed from the ground up to run Docker containers. It supports multiple orchestration tools including Kubernetes, Mesos, and Docker Swarm. There are ISOs available for installation to hardware and images for Amazon EC2, GCE, or OpenStack. You can even install RancherOS on Raspberry Pi!

RancherOS is so unique; even the system tools run in Docker. Because of this, the admin can choose a console that fits what they're comfortable with. Supported consoles are CentOS, Debian, Fedora, Ubuntu, or the default BusyBox-based console.

Rancher provides a very nice web interface for managing containers and clusters. It also makes it easy to run multiple environments including multiple orchestration suites. Rancher will be covered in more detail in Chapter 7, Using Simpler Orchestration Tools – Fleet and Cattle.

The `cloud-init` package is used to configure RancherOS. You can configure it to start containers on boot, format persistent disks, or do all sorts of other cool things. One thing it cannot do is add additional users. The idea is that there is very little reason to log in to the host once Docker is installed and configured. However, you can add SSH keys to the default rancher user to allow unique logins for different users:

```
#cloud-config
ssh_authorized_keys:
- ssh-rsa AAAAB3Nz...
```

If you need to add options to Docker, set them with `cloud-init`:

```
#cloud-config
rancher:
docker:
args: [daemon, ...]
```

### Project Atomic / RHEL Atomic

Project Atomic (http://projectatomic.io) grew out of the Fedora Project but now supports CentOS, Fedora, and RHEL. Images are available for Linux KVM and Xen-based virtualization platforms as well as Amazon EC2 and bare-metal installation.

It uses OSTree and `rpm-OSTree` to provide atomic updates. In other words, every package is updated at the same time, in one chunk. You do not have to worry that one package might have failed updates and left the system with an older package. It also provides for easy rollback in case the updates cause problems.

Project Atomic comes pre-installed with Docker and Kubernetes. (Kubernetes is covered in detail in Chapter 5, Deploying and Managing Services with Kubernetes.) This makes it an ideal base for Kubernetes-based clusters. The addition of SELinux adds an extra level of security in case one of the running containers is compromised.

