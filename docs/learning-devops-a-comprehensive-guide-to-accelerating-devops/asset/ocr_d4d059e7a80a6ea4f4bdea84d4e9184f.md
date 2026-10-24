# # Increasing the log level output

In case of errors, it is possible to add more logs during the output by adding the -v, -vvv, or -vvvv option to the Ansible command.

The -v option enables basic verbose mode, the -vvv option enables verbose mode with more outputs, and the -vvvv option adds verbose mode and the connection debugging information.

Executing the following command applies a playbook and will display more log information using the -v option that has been added:

```
`ansible-playbook -i inventory playbook.yml -v`
```

This can be useful for debugging in case of Ansible errors.

> > **Important Note**
> >The complete documentation on the `ansible-playbook` command is available here: `https://docs.ansible.com/ansible/2.4/ansible-playbook.html`.

We have just learned how to execute Ansible with its inventory and playbook by exploring some options that allow for the following:

+   - Previewing the changes that will be made by Ansible
+   - Increasing the level of logs to make debugging easier

In the next section, we will talk about data security while using Ansible Vault.

We have just seen that Ansible allows us to check a playbook before applying it to a host; it is also necessary to know that there are other tools to test the functionality of a playbook (without having to simulate its execution), such as Vagrant by HashiCorp.

Vagrant allows us to locally create a test environment composed of VMs very quickly that we can run our playbooks on and see the results. For more information on the use of Ansible and Vagrant, refer to the following documentation: `https://docs.ansible.com/ansible/latest/scenario_guides/guide_vagrant.html`.

We have just learned how to preview the changes that will be applied by Ansible. Now, let's look at how to increase the log level output of Ansible's execution.