On the Industrial Zone, we need to implement the following configurations to support this broker-service solution:

+   - Configure industrial systems to start pulling updates from the IDMZ WSUS server. This can be done via gped it .msc on each computer manually or via setting the corresponding group policy objects, as illustrated in the following screenshot:

![](bbox=[0.115, 0.22, 0.85, 0.68])

Figure 3.12 – Intranet Microsoft update service location group policy object

The following design and security consideration should be taken into account while designing and configuring this broker-service solution:

+   - Specifying the WSUS location is just one of the available settings. Make sure the update frequency, scope, and restart options are chosen to best reflect your environment as well.