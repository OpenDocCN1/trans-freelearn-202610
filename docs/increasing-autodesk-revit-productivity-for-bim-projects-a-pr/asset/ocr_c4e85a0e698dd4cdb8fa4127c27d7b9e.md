+   - **The excluding elements option:** Users should use the Exclude Members feature for minor exclusion.
+   - **Design options:** Users should avoid using groups in design options.
+   - **Levels constraints:** Users should not constrain elements to levels (for example, a top constraint of a wall).
+   - **System families:** Users should avoid using system families in groups as they are constrained to a specific level. When copied to different levels, the first level reference will be kept, and any further editing will get more complicated (remember that the further we are from the reference level, the lower the performance we will get).
+   - **Worksets:** On a worksharing project, when creating a group with elements from different worksets, Revit will ignore the existing worksets and will include all elements in the current workset when creating the group – this workset cannot be edited.
+   - **Links:** Users should not add Revit links to a group as it will be duplicated and, consequently, affect performance.

# In-place families

In-place families are bespoke families that users can create within a project context. They should not be used in a project because they will reduce performance and a standard family (that is, a loadable family) can cover most situations:

![](bbox=[0.22, 0.6, 0.74, 0.82])

Figure 1.18 – Model In-Place families