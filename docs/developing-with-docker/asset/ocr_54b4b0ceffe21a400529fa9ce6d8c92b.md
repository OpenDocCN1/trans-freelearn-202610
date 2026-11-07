# Oracle VirtualBox

Oracle VM VirtualBox is a free and open source hypervisor for x86 computers from Oracle. It will be installed by default when installing the Docker Toolbox. It supports the creation and management of virtual machines running Windows, Linux, BSD, OS/2, Solaris, and so on. In our case, the Docker Machine using VirtualBox driver, will use VirtualBox to create and boot a bitsy Linux distribution capable of the running `docker-engine`. It's worth mentioning that you can also run the teensy-weensy virtualized Linux straight from the VirtualBox itself.

Every Docker Machine you have created using the `docker-machine` or `Kitematic`, will be visible and available to boot in the VirtualBox, when you run it directly, as shown in the following screenshot:

![](bbox=[0.1, 0.36, 0.9, 0.79])

You can start, stop, reset, change settings, and read logs in the same way as for other virtualized operating systems.