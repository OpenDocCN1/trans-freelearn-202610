
## Figure 11.23 – The Load results dialog box

17. If loading is successful, you will see a screen similar to the following, showing Loading completed in the Message section:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_456_0.png)

## Figure 11.24 – Screen after loading, showing Loading completed in the Message section

![](img/b10cb271ed113c7ba5bfded0a75d13e0_456_1.png)

Congratulations! You have successfully assigned an IP address (192.168.0.1) to your PLC and loaded your PLC program and configuration to a real PLC (S7 1200, CPU 1211C).

Let’s now proceed to assign an IP address to our PC:

- 1. Right-click on the network icon on the taskbar.

![](img/b10cb271ed113c7ba5bfded0a75d13e0_457_0.png)

Figure 11.25 – Right-click on the network icon on the taskbar

- 2. Click on Open Network and Sharing Center:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_458_0.png)

Figure 11.26 – Network and Sharing Center

3. Click on Change adapter settings:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_458_1.png)

Figure 11.27 – Change adapter settings

4. Right-click on Ethernet:

Figure 11.28 – Ethernet – right-click

5. Click on Properties:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_460_0.png)

Figure 11.29 – The Ethernet Properties dialog box

6. Select Internet Protocol Version 4 (TCP/IPv4) and click on Properties:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_461_0.png)

Figure 11.30 – Internet Protocol Version 4 (TCP/IPv4) Properties – Use the following IP address selected

- 7. Select Use the following IP address and type the IP address (for example, 192.168.0.3) and subnet mask (for example, 255.255.255.0). Click on OK:

Congratulations! You have successfully assigned an IP address (192.168.0.3) with a subnet mask (255.255.255.0) to the Ethernet card of your PC.

We will now create a new project in myDESIGNER and design our screen to show the button that will act as input and a light to show the status of the pump.

Let’s get started:

3. Double-click on the project name on the left and double-click on Connections. Then, click on Add Connection:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_463_0.png)

Figure 11.33 – Project (SCADASample) – Connections

4. Click on PLC and then Add Connection:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_463_1.png)

## Figure 11.34 – Project (SCADASample) – Add Connection

5. Make the necessary settings, as shown in the following screenshot, and then click on Add:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_464_0.png)

## Figure 11.35 – Project (SCADASample) – the Add New Connection dialog box

You should see the newly added connection:

Figure 11.36 – Project (SCADASample) – a new connection is added

6. Next, double-click on Devices and click on Add Device. Configure the settings as shown in the following screenshot and then click on Test Device:

Figure 11.37 – Project (SCADASample) – adding devices

7. Click on Cancel and then Add:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_466_0.png)

Figure 11.38 – Project (SCADASample) – the connection between the PLC and the PC is tested and is okay

You should see the newly added device:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_466_1.png)

## Figure 11.39 – Project (SCADASample) – devices added

- 8. Double-click on the project title and double-click on **Tags Database**.  
  Click on **Add**:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_467_0.png)

## Figure 11.40 – Project (SCADASample) – Tags Database

- 9. Select the name of your PLC, as indicated in the following screenshot:

Figure 11.42 – Project (SCADASample) – the Add New Tag dialog box (2)

11. Click Add & Close. You should see the following screen:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_469_0.png)

Figure 11.43 – Project (SCADASample) – New tag added

12. Click Add and specify the necessary details for the tag, as shown in the following screenshot:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_470_0.png)

Figure 11.44 – Project (SCADASample) – the Add New Tag dialog box

13. Click **Add & Close**. You should see the following screen:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_470_1.png)

Figure 11.45 – Project (SCADASample) – new tag added

Congratulations! You have successfully added the necessary tags to the tags database.

Let’s now proceed to create a screen using the necessary components and tags:

- 1. Right-click on Views and select Add New:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_471_0.png)

Figure 11.46 – Project (SCADASample) – right-click on Views

- 2. Make the necessary settings, as shown in the following screenshot. Once done, click on Add:

Figure 11.47 – Project (SCADASample) – Add New View

3. Let’s now start to add components. Click on the Components Edit icon:

Figure 11.48 – Project (SCADASample) – a new view (SCADA-GUI) is added

## Figure 11.50 – Project (SCADASample – the new view (SCADA-GUI) – the Components Dialog box (2)

- 6. Expand `Siemens|MX0` and then select the tag for the switch – for example, `MX0.00@Siemens [*start]`. Then, click on **OK**:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_474_0.png)

## Figure 11.51 – Project (SCADASample) – the new view (SCADA-GUI) – specifying a tag for the component

- 7. Click on **Insert** and click on your screen:

Figure 11.52 – Project (SCADASample) – the new view (SCADA-GUI) – the Components Dialog box

8. You can select it to resize it or move it to a position of your choice on the screen:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_475_0.png)

![](img/b10cb271ed113c7ba5bfded0a75d13e0_475_1.png)

Figure 11.53 – Project (SCADASample) – the new view (SCADA-GUI) – the component added

9. Click on Component edit and then Control light in the category:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_476_0.png)

Figure 11.54 – Project (SCADASample) – the new view (SCADA-GUI) – the Components Dialog box

10. Click on ... to specify a tag. Expand QX0 and select QX0.0@Siemens [*pump], as shown in the following screenshot, and then click OK:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_477_0.png)

Figure 11.55 – Project (SCADASample) – the new view (SCADA-GUI) – specifying a tag for the component

- 11. Click on **Insert** and click on your screen:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_477_1.png)

Figure 11.56 – Project (SCADASample) – the new view (SCADA-GUI) – the Components Dialog box

12. You can now see the control light on the screen. Next, select the switch, as shown in the following screenshot:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_478_0.png)

Figure 11.57 – Project (SCADASample) – the new view (SCADA-GUI) – the switch selected

13. Click on Commands and then ...:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_478_1.png)

Figure 11.58 – Project (SCADASample) – the new view (SCADA-GUI) – adding a command to the switch (1)

14. Click Toggle and then ...:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_479_0.png)

Figure 11.59 – Project (SCADASample) – the new view (SCADA-GUI) – adding a command to the switch (2)

15. Expand the tags, as shown in the following screenshot:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_480_0.png)

Figure 11.60 – Project (SCADASample) – the new view (SCADA-GUI) – adding a command to the switch (3)

16. Select the tag MX0.0@Siemens [*start] for the switch.

![](img/b10cb271ed113c7ba5bfded0a75d13e0_480_1.png)

Figure 11.61 – Project (SCADASample) – the new view (SCADA-GUI) – adding a command to the switch (4)

17. Click on **OK** to return to your screen.

### Note

You can use the create text element tool to type text, resize it, and place it in a position of choice on your screen, as shown in the following screenshot:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_481_0.png)

Figure 11.62 – Project (SCADASample) – the new view (SCADA-GUI) – the command added and the text element created

Congratulations! You have successfully completed a screen to monitor and control the operation of a pump.

Let’s now download the project to our device:

- 1. Select your project name (SCADASample) on the left-hand side of the screen (the project tree). Click on **Download to Devices**:
