+   - **Vertex Group**: You can create a vertex group to fine-tune where you want the pressure or inflation to happen, as illustrated in the following screenshot:

![](bbox=[0.34, 0.15, 0.63, 0.31])

Figure 8.21 – Pressure vertex group

When selecting a vertex group to determine where the mesh has pressure, you may experience some strange movement. Due to the pressure on one side of the mesh, the inflated side will force the cloth to fly off in that direction. This may not be how it's intended to work but keep that in mind in case you decide to use this feature!

Now, let's move on to the Cache tab and learn how to bake the simulation!

# Cache

The Cache tab is where you bake in the simulation. It's usually a good idea to bake in the simulation before you render it, just to make sure everything works correctly, but it's not necessary.

The Cache tab may look familiar; that's because it's the same as the Soft Body Cache tab, as we can see here:

![](bbox=[0.23, 0.61, 0.74, 0.88])

Figure 8.22 – Cache