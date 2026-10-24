We have already detailed the use of HashiCorp tools in this book with Terraform and Packer, and similarly, Vault can be installed either manually or via a script, as follows:

+   • To **install Vault manually**, the procedure is exactly the same as that of installing Terraform and Packer, so you need to do the following:
+   
+   A. Navigate to the download page: `https://www.vaultproject.io/ downloads.html`.
+   
+   B. Download the package related to your OS in the folder of your choice.
+   
+   C. Unzip the package and update the `PATH` environment variable with its path to this folder.

+   • To **install Vault automatically**, we’ll use a script, the code of which depends on our OS.

For **Linux**, in a Terminal, run the following script (from `https://www. vaultproject.io/downloads`):

```
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo
apt-key add -sudo apt-add-repository "deb [arch=amd64] https://
apt.releases.hashicorp.com $(lsb_release -cs) main"sudo apt-get
update && sudo apt-get install vault
```

This script performs the following actions:

+   1. It registers to the HashiCorp package registry.
+   
+   2. Installs the Vault package.

For **Windows**, to install Vault via a script, we’ll use **Chocolatey**, which is the Windows software package manager (`https://chocolatey.org/`), by executing the following command in a Terminal:

```
choco install vault -y
```

This command downloads and installs Vault from Chocolatey.

Apart from these scripts that allow Vault to be installed on a local workstation, HashiCorp also provides Terraform code that allows you to create a complete Vault infrastructure on different cloud providers.