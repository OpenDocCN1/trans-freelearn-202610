+   7. When the installation has been completed, you may be asked to restart your laptop.
+   8. After your laptop has restarted, go back into **Settings** and ensure that these options are still selected.

If you prefer, the official Microsoft guide to turning on Hyper-V is available at `https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v`.

# Troubleshooting Docker – some obvious things

Most people will not need this section of the book. But if you do need it, then you will be reassured that you’re not the only person to meet these common issues:

+   - Check that your laptop meets the technical requirements (the type of laptop and version of Windows) for running Docker. These are listed at `https://docs.docker.com/desktop/windows/install/`. Docker will give you a clear error message if it doesn’t.
+   - When installing software, you may have seen a popup similar to the following screenshot. It means that your computer is under the control of your IT team. This is mainly a good thing (as it stops malicious software), but it does mean you need their support to install Docker:

![](bbox=[0.292, 0.455, 0.693, 0.7])

Figure AC.2 – An admin permission dialog

+   - Windows Defender sometimes blocks network and internet access – we want to allow it as we need to be able to connect and run our samples.
+   - Following a similar approach, check whether you have a connection to the internet (open a web browser) – connections can drop and cause strange errors.
+   - If you have something break halfway through an install (for example, the connection dropping or you hit the wrong button), it can be worth uninstalling Docker and starting the install process again.