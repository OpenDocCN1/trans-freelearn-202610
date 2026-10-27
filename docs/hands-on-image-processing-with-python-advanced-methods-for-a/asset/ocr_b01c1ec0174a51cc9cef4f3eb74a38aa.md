# 3D visualization with Vedo

The next code constructs colored organ meshes with the function Mesh(). Each organ receives a unique color, transparency, and interactive rendering. Two synchronized panels are displayed: CT volume rendering and Organ segmentation meshes. This enables simultaneous anatomical interpretation:

```
from vedo import Volume, Mesh, LegendBox, Plotter
LABELS={0:"background",1:"spleen",2:"r.kidney",3:"l.kidney",4:"gallbladder",
         5:"esophagus",6:"liver",7:"stomach",8:"aorta",9:"inf.vena cava",
         10:"portal vein",11:"pancreas",12:"r.adrenal",13:"l.adrenal"}
COLORS = np.array([
    [0,0,0],[255,128,0],[255,0,0],[0,0,255],[0,255,0],
    [255,255,0],[210,105,30],[255,20,147],[255,0,255],[0,255,255],
    [128,0,128],[255,165,0],[0,128,255],[255,69,0]
], dtype=np.uint8)/255.0
meshes = [] # create organ meshes
for label, name in LABELS.items():
    if label == 0: continue
    mask = seg == label
    if mask.sum() == 0: continue
    verts, faces, _, _ = measure.marching_cubes(mask, 0)
    mesh = Mesh([verts, faces])
    mesh.c(COLORS[label]).alpha(0.7)
    mesh.name = name
    meshes.append(mesh)

legend = LegendBox(meshes)
ct_vol = Volume(img) # --- CT volume ---
ct_vol.cmap("bone")
ct_vol.alpha([0,0.1,0.2,0.4,0.8])
plt = Plotter(N=2, axes=9, bg="black", size=(1600,800))
plt.show(ct_vol, at=0)  # left panel (CT)
plt.show(meshes, at=1)  # right panel (segmentation)
```

If you run the above code, you should obtain a figure like the following: