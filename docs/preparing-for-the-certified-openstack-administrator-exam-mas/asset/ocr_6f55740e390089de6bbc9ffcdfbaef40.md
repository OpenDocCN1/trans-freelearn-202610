# 7. Select Create Volume Snapshot.

![](bbox=[0.16, 0.12, 0.89, 0.46])

Figure 7.20: Setting the target container name for a Cinder backup on the Horizon dashboard

Once the backup has been created, we should have a new Swift object container with the contents of the volume compressed and archived.

We will discuss Swift in more detail in Chapter 8. For now, ensure you are scoped to the **quality-control** project and select the **Containers** panel in the **Project-Object Store** panel group. See Figure 7.21.

![](bbox=[0.08, 0.63, 0.97, 0.86])

Figure 7.21: Output from the 'openstack volume backup list' command