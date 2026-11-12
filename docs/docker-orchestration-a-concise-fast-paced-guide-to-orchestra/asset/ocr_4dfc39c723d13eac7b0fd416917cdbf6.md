When the configuration is ready, the image can be built by running `packer build <config>`, where `<config>` is the name of the configuration file. A sample is given here. Most of the output has been removed for brevity:

```
$ packer build xenial-cloudimg.json
qemu output will be in this color.
```

```
==> qemu: Downloading or copying ISO
qemu: Downloading or copying: file:///packer/images/xenial-server-cloudimg-amd64-prep.img
==> qemu: Copying hard drive...
==> qemu: Resizing hard drive...
==> qemu: Found port for communicator (SSH, WinRM, etc): 3475.
==> qemu: Looking for available port between 5900 and 6000 on 127.0.0.1
==> qemu: Starting VM, booting disk image
qemu: The VM will be run headless, without a GUI. If
you want to
qemu: view the screen of the VM, connect via VNC without a password to
qemu: 127.0.0.1:5983
==> qemu: Waiting 10s for boot...
==> qemu: Connecting to VM via VNC
==> qemu: Typing the boot command over VNC...
==> qemu: Waiting for SSH to become available...
==> qemu: Connected to SSH!
==> qemu: Provisioning with shell script: /packer/scripts/install-updates.sh
==> qemu: Provisioning with shell script: /packer/scripts/install-docker.sh
==> qemu: Gracefully halting virtual machine...
==> qemu: Converting hard drive...
Build 'qemu' finished.

==> Builds finished. The artifacts of successful builds are:
--> qemu: VM files in directory: /packer/images/xenial-cloudimg
```

![](bbox=[0.139, 0.662, 0.206, 0.738])

Ensure the user running Packer has permission to use KVM, when using the `qemu` builder. Qemu may throw permission-denied errors if the user does not have the access needed. On Ubuntu, the user needs to be in the `kvm` group.

The run will take a few minutes and, assuming everything runs properly, the new image will be placed in the directory set in the `output_directory`. Packer will not run if the output directory exists unless the `-force` option is set to avoid accidental deletion of build images.