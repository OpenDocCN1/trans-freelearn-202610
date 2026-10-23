# Detecting PLC interactions and activities

Everything we have seen so far about SilentDefense surely brings a lot of insight into the activities and traffic on the ICS network and allows us to view and monitor several aspects of the overall security posture. But the true value of the product comes from its ability to interpret the industrial protocols it sees on the network. To illustrate this ability, we are going to add an analytics tab by navigating to **Network | Analytics** and selecting **Add | Tab** from the **Network Analytics** menu bar. In the **Add tab** screen that pops up, select **From template**, specify the name as `ENIP`, and select the **ETHIP analytics** template from the **Select template** drop-down bar. The process is illustrated in the following screenshot:

![](bbox=[0.18, 0.3, 0.82, 0.6])

Figure 6.94 – Exercise 3 – Using SilentDefense – adding an ETHIP analytics tab

This creates an **Ethernet/IP analytics** tab under the **Network Analytics** screen, showing details and information such as message types over time, network activity, and **Ethernet Industrial Protocol (ENIP)** operations and commands. This is illustrated in the following screenshot: