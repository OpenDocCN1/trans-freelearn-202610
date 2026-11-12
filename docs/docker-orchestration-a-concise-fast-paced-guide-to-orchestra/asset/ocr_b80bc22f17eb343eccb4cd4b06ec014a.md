As SSH key can also be added with the `ssh-authorized-keys` option in the `users` block. You can add any number of keys to each user:

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
> 1

# RancherOS

Rancher (http://rancher.com) was designed from the ground up to run Docker containers. It supports multiple orchestration tools including Kubernetes, Mesos, and Docker Swarm. There are ISOs available for installation to hardware and images for Amazon EC2, GCE, or OpenStack. You can even install RancherOS on Raspberry Pi!