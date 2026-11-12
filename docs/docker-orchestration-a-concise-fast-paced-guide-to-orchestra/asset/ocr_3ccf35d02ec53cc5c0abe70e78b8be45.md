The conf section contains key/value pairs that will be added to the [puppetd] section of /etc/puppet/puppet.conf on the client. The server option is the address of the Puppet server. The certname option sets the name of the certificate that the client will use to authenticate. The %i and %f variables are the instance ID and the fully qualified domain name, respectively.

# Using a custom image

The problem with installing and configuring an application with configuration management or Cloud-Init is that it can lead to subtle differences in the versions of the installed software. It also takes extra time when the server first boots to install and configure everything. A faster and more consistent solution is to use an image which has been custom built for the task. CoreOS and Project Atomic are two distributions that are custom built to run Docker containers and very little else. It is also possible to build a custom image with all of the required software pre-installed.

# Building a new image with Packer

HashiCorp has created a tool called Packer, which can be used to create custom images for use with OpenStack, AWS, Azure, GCE, and VMWare. It can also create images for Xen, Linux KVM, and can even deploy to hardware via cobbleer.

A Packer configuration is a JSON file. There are three major sections, which are as follows:

+   - The builders section is a list that contains the list of image types to build.
+   - The provisioners section is a list of tasks to perform to provision an image. This includes running scripts, uploading files, and running configuration management.
+   - Finally, the post-processors section contains a list of tasks to do after the build is completed.

Those tasks can generate a manifest, run local commands, or upload an image to VMWare vSphere or Amazon S3.