![](bbox=[0.21, 0.12, 0.78, 0.48])

YARN architecture. Source: Apache Software Foundation

Containers are self-contained environments operating on top of the underlying OS; think of it like a system within a system. They allow for consistent operation of applications running inside the containers even if the underlying OS configuration and software differs between nodes. NodeManager also reports usage information back to the ResourceManager.

The ApplicationMaster has the job of negotiating resources with the ResourceManager and operating with NodeManagers to execute and monitor application tasks. The ApplicationMaster can interact with containers on different nodes to execute its application.

The ResourceManager handles the scheduling and allocation of resources to the running application based on the resource constraints of the various nodes. The ResourceManager can also restart individual application masters if there is a failure.