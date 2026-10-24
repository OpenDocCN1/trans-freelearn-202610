> Note
> If we use Ansible inside Azure Cloud Shell, we need to create this file
> (ansible.cfg) manually inside our Azure cloud drive and set the
> ANSIBLE_CONFIG environment variable with the path to the created
> file. The documentation for this environment variable is available here:
> https://docs.ansible.com/ansible/latest/reference_
> appendices/config.html#envvar-ANSIBLE_CONFIG.

The following screenshot shows an extract from this /etc/ansible/ansible.cfg
configuration file with some keys in the comments, as shown by the # symbol:

```
```
# config file for ansible -- https://ansible.com/
# ==============================================

# nearly all parameters can be overridden in ansible-playbook
# or with command line flags. ansible will read ANSIBLE_CONFIG,
# ansible.cfg in the current working directory, .ansible.cfg in
# the home directory or /etc/ansible/ansible.cfg, whichever it
# finds first

[defaults]

# some basic default values...

#inventory      = /etc/ansible/hosts
#library        = /usr/share/my_modules/
#module_utils   = /usr/share/my_module_utils/
#remote_tmp     = ~/.ansible/tmp
#local_tmp      = ~/.ansible/tmp
#plugin_filters_cfg = /etc/ansible/plugin_filters.yml
#forks          = 5
#poll_interval  = 15
#sudo_user      = root
#ask_sudo_pass  = True
#ask_pass       = True
#transport      = smart
#remote_port    = 22
#module_lang    = C
#module_set_locale = False
```
```

Figure 3.4 – Ansible configuration file
If we want to change the default Ansible configuration, we can modify this file.