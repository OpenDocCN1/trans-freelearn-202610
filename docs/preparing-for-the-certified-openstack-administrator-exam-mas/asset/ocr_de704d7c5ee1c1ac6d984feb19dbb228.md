# Exam objective - downloading images

Downloading images stored in Glance is not possible via the Horizon dashboard in the Newton version of OpenStack. We will rely on the python-openstackclient to accomplish this.

# CLI

Verify you are scoped to the procurement project by sourcing the openrc-procurement file created in the previous section:

```
$ source openrc-procurement
```

List all available images:

```
$ openstack image list
```

Download the cirros-cli image to the /tmp directory inside your virtual appliance. You can also use the image's UUID instead of the name.

```
$ openstack image save --file /tmp/mydownloadedimage.img cirros-cli
```

Verify the image successfully downloaded to the /tmp location:

```
$ ls /tmp
```

# Exam objective - sharing images with specific projects

An OpenStack user with the admin role has the ability to share an image with a specific project. As of the Newton version of OpenStack, Horizon does not have the capability to share images with specific projects. We will do this via python-openstackclient.

# CLI

Ensure you are scoped to the procurement project:

```
$ source openrc-procurement
```

Get a list of all images we currently have access to:

```
$ openstack image list
```