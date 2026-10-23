# Emulated OPC network stack

Follow these instructions to install the CIP stack on the Ubuntu machine you built for the OPC emulation:

+   1.  Log into the Ubuntu machine and open a terminal.
+       2.  Run the `sudo apt install python-pip python3-dev` command to install the required Python packages.
+       3.  Switch to an elevated (`sudo`) terminal with `sudo su` since the port the OPC server will run on is in the lower range of port numbers (below `1000`), which requires root privileges to assign.
+       4.  Run the `pip3 install --upgrade pip ipython` command to install the required Python dependencies.
+       5.  Install the OPC Python module with `pip install opua`.
+       6.  We will now use `ipython` to run the server. Start `ipython` with the `ipython` command. Then, enter the following commands to define and start the OPC server:

```
```
import opua
server = opua.server.server.Server()
server.start()
```
```

The output is shown in the following screenshot:

![](bbox=[0.12, 0.54, 0.83, 0.77])

Figure 19.10 – Starting the OPC-UA server

+   7.  The server is now ready to receive requests.

For additional documentation on the `python-opcua` Python OPC implementation, refer to the project’s GitHub repository at `https://github.com/FreeOpcUa/python-opcua`.