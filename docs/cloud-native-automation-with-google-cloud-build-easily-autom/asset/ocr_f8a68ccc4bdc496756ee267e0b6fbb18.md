# Using private pools

Private pools are similar to the default pool in that they are fully managed workers that live in a Google-managed environment. Designed for more complex sets of requirements from those looking to run fully managed builds, private pools help users who require the following:

+   - To run resources on a private VPC network that must be accessed from a build
+   - More optionality around machine types, such as high-memory machines
+   - To further secure the network perimeter of their build environment

Unlike the default pool, you can have multiple private pools in a single Google Cloud project. You create empty private pool resources that contain a definition of what a private pool worker should look like, with details such as their machine type and the network they should connect to. Google will then scale up these private pools from zero workers when builds are scheduled to run on them.

With that said, the workflow for end users submitting builds remains consistent, as we can see here:

![](bbox=[0.14, 0.43, 0.87, 0.88])

Figure 2.6 – Delineation between what users do and what Cloud Build does when using private pools