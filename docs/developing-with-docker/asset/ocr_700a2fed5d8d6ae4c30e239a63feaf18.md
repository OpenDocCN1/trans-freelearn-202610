# Hardware requirements

To use Docker, you will need some reasonably new machine, that supports hardware-level virtualization: VT-x for Intel-based PCs and AMD-V for AMD processors. Most of the Mac machines support it out of the box, but for PC you will need to make sure it's turned on and perhaps enable it in the BIOS settings. It will be different for different BIOS, just look for VT-x / AMD-V switch.

In Windows 8, you can check virtualization support in the task manager in the Performance tab:

|  |  |
|---|---|
| Virtualization: | Enabled |
| L1 cache: | 256 KB |
| L2 cache: | 2.0 MB |

If you want to check whether your PC supports the hardware level virtualization in Windows 7, look for Microsoft Hardware-Assisted Virtualization Detection Tool. Its free, tiny utility is used to check whether your system supports virtualization. Download and run it to see the report:

![](bbox=[0.22, 0.47, 0.78, 0.8])