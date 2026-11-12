![](bbox=[0.126, 0.09, 0.866, 0.425])

Figure 3.16 – Construction Script panel

**Construction Script** is very useful for creating flexible Blueprints that allow the Level Designer to configure some features of an instance of these Blueprints within the Level Editor.

As an example, let's create a Blueprint with an **Instance Editable Static Mesh** so we can choose a different Static Mesh for each instance of the Blueprint that is on the Level:

+   1. Create or use an existing project that has the starter content.
+   2. Click the **Add** button in **Content Browser** and choose the **Blueprint Class** option.
+   3. On the next screen, choose **Actor** as the parent class.
+   4. Name the Blueprint `BPConstruction` and double-click it to open the Blueprint Editor.
+   5. Click the **Add** button in the **Components** panel and choose the **Static Mesh** Component. Rename the Component `StaticMeshComp`, as shown in the following screenshot: