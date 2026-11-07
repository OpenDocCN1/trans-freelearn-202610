In the past, it was customary to download your favorite operating system as an ISO, a single archive file of an optical disc. After getting your virtual machine booted, you perhaps provided answers during an install menu and went through an install wizard.

It's important to understand that in the elastic cloud world, agility is the priority and we rarely waste time with installers! When one boots an instance in an OpenStack environment, it is always required to provide a disk image (we can alternately supply an existing volume or Nova snapshot, but more on that later). Nova then proceeds to send an API request to retrieve the image from Glance. The image is placed on the compute node and the virtual machine is booted.

Unlike a typical ISO, cloud images that exist in Glance are typically snapshots of a disk's contents. These images have been previously configured by a person or script that has gone through the initial installation procedure and has installed specific programs and configuration files to ensure it is cloud aware.

Glance can store these images in a variety of backends called **data stores**. Glance supports a variety of data stores, including the local filesystem, NFS, or an OpenStack Swift container.

# Building cloud images

Although outside the scope of the COA exam, it's important that we understand that the process of building these cloud images is done *outside* of OpenStack. Let's take a quick look at a few popular methods for creating a cloud image:

+   - **OpenStack disk image builder project**: The OpenStack disk image builder project is a tool for automatically building customized operating system images for use in clouds. It includes support for building images based on Ubuntu, Red Hat, CentOS, SUSE, and many others (https://github.com/openstack/diskimage-builder).
+   - **Hashicorp Packer**: Packer automates the building of any type of image and connects into existing configuration-management frameworks such as Chef and Puppet (packer.io).
+   - **Oracle VirtualBox**: Although extremely tedious, you can use Oracle VirtualBox to build cloud images by setting up a virtual machine to your desired state and uploading it into Glance (virtualbox.org).