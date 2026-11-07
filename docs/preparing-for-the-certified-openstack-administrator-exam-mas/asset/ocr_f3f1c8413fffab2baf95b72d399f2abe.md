You should see Trove in the service catalog output:

```
+-------------------------------+
| Name | Type | Endpoints |
+-------------------------------+
| trove | database | RegionOne |
| | | Internal: http://192.168.56.56:8779/v1.0/ |
| | | RegionOne |
| | | admin: http://192.168.56.56:8779/v1.0/ |
| | | RegionOne |
| | | public: http://192.168.56.56:8779/v1.0/ |
+-------------------------------+
```

Figure 3.28: The service catalog should now advertise the newly added Trove service and public, internal, and admin endpoints

![](bbox=[0.1, 0.32, 0.18, 0.38])

If you add a new service and public endpoint via the CLI, it will not show up on the Horizon dashboard until you sign out and sign back in with your username and password.

# Summary

You should now have a solid understanding of Keystone and the Keystone-centric objectives required for the COA exam. Remember the importance of changing one's project scope via the Horizon dashboard and CLI. We need to use the python-openstackclient when creating any new domains outside of default and placing projects, users, and groups in those domains. We will also need the CLI when adding or removing entries from the service catalog.

In the next chapter, we will use discuss the Glance image service and work through all Glance exam objectives via the CLI and Horizon dashboard. Let's go!