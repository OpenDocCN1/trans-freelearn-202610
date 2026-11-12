# # Using Puppet with Packer

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

# # Treating infrastructure like code

Let’s pause for a moment and consider what was just done. The server has been completely defined in one or more files with a scriptable build process. We have just turned our servers into code.