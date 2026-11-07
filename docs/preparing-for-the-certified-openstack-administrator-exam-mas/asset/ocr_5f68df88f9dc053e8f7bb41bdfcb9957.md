# Horizon dashboard

See Figure 8.4. Let's create a new container on the Horizon dashboard.

![](bbox=[0.015, 0.15, 0.91, 0.36])

Figure 8.4: Creating a Swift container on the Horizon dashboard

+   1. From the OpenStack overview page, scope yourself to the **operations** project by selecting the top dropdown.
+   2. Select the **Containers** panel from the **Project-Object Store** panel group.
+   3. Select **Container**.
+   4. See Figure 8.5. **Container Name (required)**: A name for your container. Enter `container-dashboard`.
+   5. **Container Access Public (required)**: Makes the contents of this container accessible to the public. This means anyone in the world can browse to any of the objects within the container (as long as the swift API endpoint is publicly accessible). Lets keep this **unchecked**, thus making the container private. By default, a private container's objects are only accessible to users with the **admin** or **swiftoperator** roles. We will learn more about ACLs in the upcoming section.