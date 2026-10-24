This command displays the installed Packer version:

```
PS C:\Users\mkrief> packer --version
1.7.3
```

Figure 4.4 – Packer version command

To see all of the Packer command-line options, we can execute the following command:

```
packer --help
```

After executing, we will see a list of available commands, as shown in the following screenshot:

```
PS C:\Users\mkrief> packer --help
Usage: packer [--version] [--help] <command> [<args>]

Available commands are:
    build        build image(s) from template
    console      creates a console for testing variable interpolation
    fix          fixes templates from old versions of packer
    fmt          Rewrites HCL2 config files to canonical format
    hcl2_upgrade transform a JSON template into an HCL2 configuration
    init         Install missing plugins or upgrade plugins
    inspect      see components of a template
    validate     check that a template is valid
    version      Prints the Packer version
```

Figure 4.5 – Packer help command

We have just seen the manual installation procedure for Packer and installation with
a script on different OSes, as well as its integration with Azure Cloud Shell.

We will now write a template to create a VM image in Azure with Packer using scripts.

# # Creating Packer templates for Azure VMs
with scripts

As mentioned in the introduction, to create a VM image, Packer is based on a file
(template) that is written in JSON format or in **HashiCorp Configuration Language**
(HCL), which was introduced in Packer from version 1.5.0 (read the following blog post
for more information: `https://www.packer.io/guides/hcl`). We will first see
the structure and composition of this template, and then we will put into practice how to
create a template that will create a VM image in Azure.