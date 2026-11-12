Deployment on almost any local cloud system or EC2 is made easier by the use of cloud-init. The cloud-init package lets you configure your Atomic hosts automatically on boot, instantly growing your Kubernetes cluster.

You can use cloud-init to set the password and enable SSH logins for the default user:

```
#cloud-config
password: c001-pa$$word
ssh_pwauth: True
```

You can also add SSH keys to the default user's authorized_keys file:

```
#cloud-config
ssh_authorized_keys:
  - ssh-rsa AAAAB3Nz...
  - ssh-rsa AAAAB3Nz...
```

> The name of the default user depends on which version of Atomic you use. For Fedora, the username is fedora, for CentOS it is centos, and for RHEL it is cloud-user.

# # Running single container applications

Before we get into the nuts and bolts of Docker orchestration, let's run through the basics of running single applications in Docker. Seeing as this is a tech book, the first example is always some variant of Hello World and this is no different.

> By default, docker must be run as the root user or with sudo. Instead, you could add your user to the docker group and run containers without root.

```
$ docker run --rm ubuntu echo "Hello World"
```

This example is really simple. It downloads the ubuntu Docker image and uses that image to run the echo "Hello World" command. Simple, right? There is actually a lot going on here that you need to understand before you get into orchestration.