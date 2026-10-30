![](bbox=[0.176, 0.148, 0.823, 0.348])

+   5. Select **Enable Camera**.
+   6. Select **Advanced Options**.
+   7. Under this option, select **Memory Split** and enter 64MB for GPU.

This option decides how much RAM is used by the **GPU (Graphics Processor Unit)**. The more RAM is allocated to the GPU, the more intensive graphics processing can be done. 64 MB is a good value for most graphics purposes.

Once all these options are modified, select **Finish**. This will prompt for a reboot of the Pi. Choose **Yes** and let it reboot. Once rebooted, it will automatically take us to the Raspbian Desktop again.

You can always invoke the `raspi-config` tool from Command Prompt with the following command and change the settings:

```
sudo raspi-config
```

# Shutting down and rebooting Pi safely

In the Raspbian menu, there are options to shut down and reboot the Pi.

If we click on the following **Menu** button on the desktop, it will display multiple options:

![](bbox=[0.443, 0.738, 0.557, 0.77])