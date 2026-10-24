+   3. Create an SSH connection with SSH keys (Vagrant creates private/public SSH keys).
+   4. Mount the shared folder.
+   5. Apply the provisioning Ansible script.

After the provisioning of the VM, we can view this VM in the hypervisor here in
VirtualBox.

Now that the VM is up and has been provisioned, we can connect it.

# ## Connecting to the VM

To connect the VM with SSH, we can use the default SSH command dedicated to Vagrant
(in administrator mode):

```
`vagrant ssh`
```

The output is shown in the following screenshot:

```
PS C:\Users\Administrator\Learning-DevOps-Second-Edition\CHAP05\VagrantFiles> vagrant ssh
Welcome to Ubuntu 18.04.5 LTS (GNU/Linux 4.15.0-154-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

System information as of Sun Aug 29 14:25:23 UTC 2021

System load:  0.01                Processes:             96
Usage of /:   4.1% of 38.71GB     Users logged in:       0
Memory usage: 17%                 IP address for enp0s3: 10.0.2.15
Swap usage:   0%

0 updates can be applied immediately.

New release '20.04.3 LTS' available.
Run 'do-release-upgrade' to upgrade to it.

vagrant@ubuntu-bionic:~$
```

Figure 5.11 – Connecting SSH to the Vagrant VM

The execution of this command means that Vagrant automatically connects the SSH
credential to the VM, and we can run any commands or scripts inside the VM.