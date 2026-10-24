

> Note
> In this section, we learned how to create a VM using a Vagrant configuration file. If you wish to create multiple VMs in the same configuration file, please refer to the official documentation at https://www.vagrantup.com/docs/multi-machine.

We have just discussed how to choose a Vagrant box and how to write a Vagrant configuration file. Now, in the next section, we will learn how to use the Vagrant CLI and the configuration file to create a VM locally.

## Creating a local VM using the Vagrant CLI

Now that we have written the configuration file, we can create our VM locally.

To perform this, we will use several Vagrant CLI commands. To display all of the available commands, we will run the `vagrant --help` command:

```
PS C:\Learning-DevOps-Second-Edition\CHAP05\VagrantFiles> vagrant --help
Usage: vagrant [options] <command> [<args>]

-h, --help Print this help.

Common commands:
 autocomplete manages autocomplete installation on host
 box manages boxes: installation, removal, etc.
 cloud manages everything related to Vagrant Cloud
 destroy stops and deletes all traces of the vagrant machine
 global-status outputs status Vagrant environments for this user
 halt stops the vagrant machine
 help shows the help for a subcommand
 init initializes a new Vagrant environment by creating a Vagrantfile
 login
 package packages a running vagrant environment into a box
 plugin manages plugins: install, uninstall, update, etc.
 port displays information about guest port mappings
 powershell connects to machine via powershell remoting
 provision provisions the vagrant machine
 push deploys code in this environment to a configured destination
 rdp connects to machine via RDP
 reload restarts vagrant machine, loads new Vagrantfile configuration
 resume resume a suspended vagrant machine
 snapshot manages snapshots: saving, restoring, etc.
 ssh connects to machine via SSH
 ssh-config outputs OpenSSH valid configuration to connect to the machine
 status outputs status of the vagrant machine
 suspend suspends the machine
 up starts and provisions the vagrant environment
 upload upload to machine via communicator
 validate validates the VagrantFile
 version prints current and latest Vagrant version
 winrm executes commands on a machine via WinRM
 winrm-config outputs WinRM configuration to connect to the machine
```

Figure 5.9 – Displaying the Vagrant commands

> Note
> All details about the Vagrant CLI commands are documented at https://www.vagrantup.com/docs/cli.

In the next section we will learn how to create the VM, then we will connect to this VM, and finally we will perform some scripts.

## Creating the VM

To create the VM, navigate to the folder in which we created the `Vagrantfile` file, and run the following command:

```
vagrant up
```

The output is shown in the following screenshot:

![](img/d3344b39c888aab0b832625465f837c9_170_0.png)

The execution of the preceding command performs the following steps:

- Import the box from Vagrant Cloud.
- Create a new VM in the hypervisor (in our case, this is VirtualBox).
- Create an SSH connection with SSH keys (Vagrant creates private/public SSH keys).
- Mount the shared folder.
- Apply the provisioning Ansible script.

After the provisioning of the VM, we can view this VM in the hypervisor here in VirtualBox.

Now that the VM is up and has been provisioned, we can connect it.

## Connecting to the VM

To connect the VM with SSH, we can use the default SSH command dedicated to Vagrant (in administrator mode):

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

The execution of this command means that Vagrant automatically connects the SSH credential to the VM, and we can run any commands or scripts inside the VM.

For example, we can run the `ansible --version` command to display the installed version of Ansible:

```
vagrant@ubuntu-bionic: $ ansible --version
ansible 2.9.24
  config file = /etc/ansible/ansible.cfg
  configured module search path = [u'/home/vagrant/.ansible/plugins/modules', u'/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python2.7/dist-packages/ansible
  executable location = /usr/bin/ansible
  python version = 2.7.17 (default, Feb 27 2021, 15:10:58) [GCC 7.5.0]
vagrant@ubuntu-bionic: $
```

Figure 5.12 – Executing commands inside the VM

After running the desired tests, if we wish to destroy this VM to remake it, we can run the `vagrant destroy` command:

```
PS C:\Users\<username>\Learning-DevOps-Second-Edition\CHAP05\VagrantFiles> vagrant destroy
default: Are you sure you want to destroy the 'default' VM? [y/N] y
==> default: Forcing shutdown of VM...
==> default: Destroying VM and associated drives...
```

Figure 5.13 – Destroying the Vagrant VM

In this section, we learned how to create a VM and connect to it using the Vagrant CLI.

## Summary

In this chapter, we learned that it was possible to create VMs locally using Vagrant from HashiCorp in order to have an isolated development environment.

We discussed how to download and install it. Then, we learned how to write a Vagrant configuration file using a Bionic box, an `ansible install` script, and a locally shared folder.

Finally, we learned how to execute the Vagrant command lines in order to create this VM and connect to it for testing purposes.

In the next chapter, we will start a new topic, which is the implementation of CI/CD, and we will learn how to use Git to source your code.

## Questions

- What is the role of Vagrant?
- What is the Vagrant command to create a VM?
- What is the Vagrant command to connect SSH to a VM?

## Further reading

If you want to know more about Vagrant, please take a look at the following resources:

- The official Vagrant documentation: https://www.vagrantup.com/docs/
- The official Vagrant Cloud documentation: https://www.vagrantup.com/vagrant-cloud

## Section 2: DevOps CI/CD Pipeline

This part covers the DevOps pipeline process, starting with the principles of continuous integration and continuous deployment. It will be illustrated through the use of different tools, including Jenkins, Azure Pipelines, and GitLab.

This section comprises the following chapters:

- Chapter 6, Managing Your Source Code with Git
- Chapter 7, Continuous Integration and Continuous Deployment
- Chapter 8, Deploying Infrastructure as Code with CI/CD Pipelines

## 6 Managing Your Source Code with Git

A few years ago, when we were developers and writing code as part of a team, we encountered recurring problems that were for the most part as follows:

- How to share my code with my team members
- How to version the update of my code
- How to track changes to my code
- How to retrieve an old state of my code or part of it

Over time, these issues have been solved with the emergence of source code managers, also called a **version control system (VCS)** or noted more commonly as a **version control manager (VCM)**.

The goals of these VCSs are mainly to do the following:

- Allow collaboration of developers' code
- Retrieve the code
- Version the code
- Track code changes

