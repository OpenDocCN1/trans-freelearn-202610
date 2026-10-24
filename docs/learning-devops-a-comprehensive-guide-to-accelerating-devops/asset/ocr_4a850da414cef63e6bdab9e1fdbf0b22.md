To translate the same block code in HCL format, we write first two `sources` block
that define the different provider properties. And we call these `sources` in the `build`
section as follows:

```
build {
    sources = ["sources.azure-arm.azurevm","sources.docker.
docker-img"]
    ...
}
```

In this code sample, we define in the Packer template the information for an image of an
Azure VM, and the information for a Docker image based on Alpine. The advantage of
this is to standardize the scripts that will be detailed in the provisioning section of these
two images.

Just after the details of each section, we will see a concrete example with a `builders`
section to create an image in Azure.

Let’s move on to explaining the `provisioners` section.

# The provisioners section

The `provisioners` section, which is optional, contains a list of scripts that will be
executed by Packer on a temporary VM base image in order to build our custom VM
image according to our needs.

If the Packer template does not contain a `provisioners` section, no configuration
will be made on the base images.

The actions defined in this section are available for Windows as well as Linux images,
and the actions can be of several types, such as executing a local or remote script,
executing a command, or copying a file.

> Note
> The `provisioners` type proposed natively by Packer is detailed
> in the documentation: https://www.packer.io/docs/
> provisioners/index.html.

It is also possible to extend Packer by creating custom provisioning types. To learn more
about custom `provisioners`, refer to the documentation here: https://www.
packer.io/docs/extending/custom-provisioners.html.