
base_kwargs = dict(in_channels=1, out_channels=14, feature_size=48)
model = SwinUNETR(**base_kwargs).to(device)
ckpt = torch.load(f"{BUNDLE_DIR}/models/model.pt", map_location=device)
model.load_state_dict({k.replace("module.",""): v for k,v in ckpt.items()},\
                    strict=False)

model.eval()
preprocess = Compose([EnsureChannelFirst(channel_dim="no_channel"),
    Orientation(axcodes="RAS"),
    Spacing(pixdim=(1.5,1.5,2.0), mode="bilinear"),
    ScaleIntensityRange(a_min=-175, a_max=250, b_min=0., b_max=1., clip=True),
    EnsureType()])

def run_inference(ct_path):
    nii = nib.load(ct_path)
    img_t = preprocess(nii.get_fdata()).astype(np.float32)
    img_padded, orig_shape = pad32(img_t) # refer to github for pad32
    with torch.no_grad():
        pred = sliding_window_inference(img_padded.unsqueeze(0).to(device),
            roi_size=(96,96,96), sw_batch_size=2, predictor=model, overlap=0.5)
    H,W,D = orig_shape
    seg = torch.softmax(pred,1).argmax(1).squeeze()[:H,:W,:D].cpu().numpy()
    return img_t.squeeze().cpu().numpy(), seg

ct_path = os.path.join(INPUT_DIR,"liver_3.nii.gz")
img, seg = run_inference(ct_path)
# Plot img, seg, overlay side-by-side here

If you run the above code and plot the images side-by-side, you should obtain a figure like the following:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_579_0.png)

Figure 11.17 – MONAI: Swin UNETR axial segmentation of a liver CT image

## 3D visualization with Vedo

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

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_581_0.png)

## Volume rendering with VTK

VTK performs GPU-accelerated volumetric rendering. The CT volume is represented as: `VtkImageData`. Opacity transfer functions define tissue transparency with the function `opacity.AddPoint()`. Color transfer functions map intensity values to tissue colors. The rendered image approximates radiological visualization systems are used clinically. You can rotate the camera with the method `camera.Azimuth(2)` for 180 frames. Each rendered frame is captured and stored. Finally, `imageio.mimsave()` can be used to create an animated 3D rotation of the segmented organs. Refer to GitHub for the code.

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_581_1.png)

## Why Swin-UNETR performs well

Swin-UNETR achieves strong performance because it combines the following components, as shown in the following table:

| Component | Advantage |
|---|---|
| Swin Transformer | Long-range context modeling |
| Shifted windows | Efficient attention |
| U-Net decoder | Precise localization |
| Skip connections | Recovery of spatial details |
| Hierarchical features | Multi-scale understanding |
| Pretraining | Improved generalization |

Table 11.1 – Key architectural components of Swin-Unet and their corresponding advantages for image segmentation

Having understood the architectural design choices that enable Swin-UNETR to effectively model complex 3D anatomical structures, we now connect these strengths to their practical impact in real-world medical imaging scenarios.

## Applications of 3D organ segmentation

Transformer-based volumetric segmentation has numerous applications, such as liver tumor analysis, radiotherapy planning, surgical navigation, organ volume estimation, disease progression tracking, multi-organ anatomical modeling, digital twin generation, robotic surgery assistance, etc. Unlike classical CNN approaches, Swin-UNETR leverages hierarchical self-attention to capture long-range anatomical dependencies across volumetric CT data, producing highly accurate organ segmentation maps suitable for modern clinical AI systems.

## Remote sensing: Multi-source satellite imagery segmentation with mixed-attention U-Net

Semantic segmentation of satellite imagery is one of the most important tasks in modern remote sensing and geospatial intelligence systems. Unlike natural-image segmentation, remote sensing imagery contains large-scale spatial structures, severe class imbalance, seasonal appearance variations, atmospheric distortions, and multi-source acquisition artifacts originating from different sensors and satellites. Accurate segmentation of roads, buildings, vegetation, water bodies, barren land, and urban regions is therefore critical for applications such as urban planning, precision agriculture, disaster management, military surveillance, environmental monitoring, and autonomous navigation.

In this section, a Mixed-Attention U-Net (MAnet) architecture from the segmentation_models_pytorch library is trained for multi-class semantic segmentation of satellite images. The network employs a pretrained ResNet34 encoder together with attention-guided decoder blocks to improve contextual feature aggregation and boundary localization.

The complete pipeline includes:

- 1. Patch extraction from large satellite images
- 2. RGB-mask encoding into categorical labels
- 3. Class-balanced loss computation
- 4. Data augmentation using Albumentations
- 5. Mixed Dice + Cross-Entropy optimization
- 6. Mixed-Attention U-Net training
- 7. Full-image tiled inference and reconstruction
- 8. Quantitative evaluation using Dice coefficient

## Semantic segmentation in remote sensing

Let the input satellite image be $I: \Omega \to \mathbb{R}^3$, where $\Omega \subset \mathbb{Z}^2$ represents the image lattice. The goal of semantic segmentation is to estimate a dense categorical mapping $S: \Omega \to \{0,1, ..., C-1\}$, where $C$ denotes the number of semantic land-cover classes.

For a pixel $(x, y)$, $S(x, y) = c$ means that the pixel belongs to the class $c$, such as road, vegetation, water, urban area, barren land, buildings, and shadows. Unlike image classification, segmentation performs pixel-level prediction, requiring both local texture understanding and global contextual reasoning.

## Challenges in satellite image segmentation

Remote sensing segmentation presents several unique challenges, such as the following:

- **Large spatial resolution:** Satellite images may contain dimensions exceeding several thousand pixels:
  
  $H, W \gg 1024$
  
  Directly processing such images is computationally expensive because GPU memory grows approximately as $O(HW)$ for convolutional feature maps.
- **Severe class imbalance:** Certain classes dominate the image. For example, background or vegetation may occupy (80%) of pixels, roads or rivers may occupy less than (1%). This causes biased optimization because standard cross-entropy minimizes average error:
  
  $\mathcal{L}_{CE} = - \sum_{c=1}^{C} y_c \log p_c$
  
  Frequent classes dominate the gradients.
- **Multi-source variability:** Images originate from different satellites, sensors, altitudes, atmospheric conditions, and acquisition times. Thus, the same land-cover class may exhibit dramatically different spectral appearance.
