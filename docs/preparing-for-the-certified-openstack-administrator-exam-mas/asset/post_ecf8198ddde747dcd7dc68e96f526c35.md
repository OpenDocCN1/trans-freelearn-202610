
We should now see the output shown in Figure 4.11:

![](img/da864e0670c65edf6f5ab861ef9906b3_134_0.png)

Figure 4.11: Output from the 'openstack image create' command

Use this to verify that the image was successfully created by using the openstack image list command:

```
$ openstack image list
```

You should see output similar to Figure 4.12 with all images set to active, verifying the image is available to be used:

![](img/da864e0670c65edf6f5ab861ef9906b3_134_1.png)

Figure 4.12: Output from the 'openstack image list' command

If we wanted to delete this image, we could easily use the simple openstack image delete command. See openstack help image delete for more information.

## Exam objective - downloading images

Downloading images stored in Glance is not possible via the Horizon dashboard in the Newton version of OpenStack. We will rely on the python-openstackclient to accomplish this.

### CLI

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

## Exam objective - sharing images with specific projects

An OpenStack user with the admin role has the ability to share an image with a specific project. As of the Newton version of OpenStack, Horizon does not have the capability to share images with specific projects. We will do this via python-openstackclient.

### CLI

Ensure you are scoped to the procurement project:

```
$ source openrc-procurement
```

Get a list of all images we currently have access to:

```
$ openstack image list
```

Get a list of all the projects in our environment so you can lookup the UUID of the marketing project:

```
$ openstack project list
```

Share the cirros-cli image with the marketing project:

```
$ openstack image add project cirros-cli 99a06694e4444e038d632aca0cc1a89e
```

After sharing the image, you should see the output similar to Figure 4.13:

![](img/da864e0670c65edf6f5ab861ef9906b3_136_0.png)

Figure 4.13: Output from the 'openstack image add' command

![](img/da864e0670c65edf6f5ab861ef9906b3_136_1.png)

Although you'll see a pending status, anyone in the marketing project should now be able to see and launch the shared image. Accepting the image is not necessary with the Glance v2 API.

Any non-admin user in the marketing project will now be able to launch an instance or create a volume from the image but will not be able to delete the image or modify the properties/metadata associated with it.

## Exam objective - setting Glance image properties and metadata definitions

It's extremely easy to set Glance image properties and metadata definitions on images that reside in Glance. Remember that you can only set metadata on images owned by the project to which you are currently scoped.

### Horizon dashboard

Recall earlier in this section, where we pointed out the two specific sections for setting Glance image properties and metadata definitions.

![](img/da864e0670c65edf6f5ab861ef9906b3_137_0.png)

Figure 4.14: Editing Glance image properties on the Horizon dashboard

See Figure 4.14. Let's first update the image properties associated with the `cirros-dashboard` image:

- 1. Verify you are scoped to the procurement project.
- 2. Select the **Images** panel from the **Project-Compute** panel group.
- 3. Click on the `cirros-dashboard` dropdown next to the **Launch** button and select **Edit Image**.
- 4. See Figure 4.15. Lower the minimum RAM value to 256.
- 5. Click on **Update Image**.

![](img/da864e0670c65edf6f5ab861ef9906b3_138_0.png)

Figure 4.15: Editing Minimum Ram for a Glance image on the Horizon dashboard

Let's now apply a Glance metadata definition key/value to the image. Let's use the existing *Common Operating Systems Properties-OS Distro* metadata definition property. This will be used to help identify the specific distribution contained in the image. See *Figure 4.16*.

- 1. Verify you are scoped to the procurement project.
- 2. Select the **Images** panel from the **Project-Compute** panel group.
- 3. Click on the `cirros-dashboard` dropdown next to the **Launch** button and select **Update Metadata**.

![](img/da864e0670c65edf6f5ab861ef9906b3_139_0.png)

Figure 4.16: Updating Glance image metadata on the Horizon dashboard

- 4. See *Figure 4.17*. Select the plus sign on the left to open the **Common Operating System Properties** group. Select **OS Distro** definition. Fill in the value as `Cirros`. See *Figure 4.16*. Take note that the description we originally set on the image is a Glance metadata definition key/value pair.
- 5. Select **Save**.

### Update Image Metadata

You can specify resource metadata by moving items from the left column to the right column. In the left column there are metadata definitions from the Glance Metadata Catalog. Use the 'Custom' option to add metadata with the key of your choice.

- **Available Metadata**
- - Custom
- - CiM Processor Allocation Setting
- - Cinder Volume Type
- - Common Operating System Properties
- - OS Admin User
- - OS Version
- - CPU Pinning
- - Database Software
- - Guest Memory Backing

- **Existing Metadata**
- - description: Cirros Cloud Im...
- - os_distro: Cirros

### OS Distro (os_distro)

The common name of the operating system distribution in lowercase. Uses the same data vocabulary as the libosinfo project. Specify only a recognized value for this field. Deprecated values are listed to assist you in searching for the recognized value.

To verify your work, click on the blue hyperlink for the cirros-dashboard in the image list page. See Figure 4.18. You should see the newly updated Min. RAM value set to 256 MB and the os_distro value set to Cirros.

![](img/da864e0670c65edf6f5ab861ef9906b3_141_0.png)

Figure 4.18: A Glance image with updated Min. RAM (image properties) and os_distro (metadata applied from metadata definitions)

### CLI

Ensure you have sourced credentials for the procurement project:

```
$ source openrc-procurement
```

Verify you can get a list of all available images:

```
$ openstack image list
```

Let's set the minimum RAM requirements on the cirros-cli image to 256 MB. You will not receive any confirmation output from this command:

```
$ openstack image set cirros-cli --min-ram 256
```

Let's now apply the Glance metadata definition os_distro key/value to the image:

```
$ openstack image set cirros-cli --property os_distro='Cirros'
```

We can now verify that the Glance property and metadata definitions were properly set to the image. We should see output similar to Figure 4.19.

```
$ openstack image show cirros-cli
```

![](img/da864e0670c65edf6f5ab861ef9906b3_142_0.png)

Figure 4.19: An image with updated Min. RAM and os_distro properties on the CLI

## Summary

The Glance image service is one of the easier OpenStack services to understand and use. We've covered the basics of cloud images, image file types, Glance architecture as well as image properties and metadata definitions. It's important to remember that the only user that can make an image public (publicly accessible to all projects across all domains) must have the admin role. A user with the admin role can also share an image with a specific project. If a user does not have the admin role, they cannot modify or delete existing images unless that image was created by someone within the project to which they are currently scoped.

In the next chapter, we will discuss the first part of the Nova compute service and work through all objectives relating to key pairs, flavors, and the launching of virtual machines on the Horizon dashboard and python-openstackclient.
