Similarly, to segment an image from Cityscapes run the following code:

```
segment_image(cityscapes_image, processor=city_processor, model=city_model)
```

If you run the above code, you should obtain a figure like the following:

![](bbox=[0.125, 0.2, 0.905, 0.33])

Figure 10.35 – Segmenting a Cityscape image with pretrained OneFormer

The above code demonstrates real-world datasets, introduces standardized evaluation benchmarks and prepares groundwork for universal segmentation pipelines. OneFormer learns to segment anything, anywhere, under any task, with a single unified transformer.

Key takeaways: The last part unified classical, CNN-based, and transformer-based segmentation under a common perspective, highlighting how methods evolve from pixel/region modeling to object-centric and query-based representations. It emphasized the core distinctions and connections between semantic, instance, and panoptic segmentation across modern architectures.

# Summary

In this chapter, we established a strong foundation in image segmentation, progressing from classical concepts to modern deep learning and transformer-based methods. We explored the fundamental formulations of semantic, instance, and panoptic segmentation, and examined how architectures such as CNNs and transformers (e.g., DPT, SegFormer, DETR, Mask2Former) model spatial context, global dependencies, and object-level reasoning. Through both theoretical insights and practical implementations, the reader has gained a clear understanding of how segmentation models evolve from pixel-level classification to unified, query-based frameworks. This foundation prepares us to move toward more advanced paradigms in the next part, where segmentation becomes increasingly interactive, multimodal, and application driven.

In Chapter 11, More Deep Learning Methods for Image Segmentation, we will explore promptable foundation models, vision–language and text-guided segmentation, monocular depth estimation and 3D scene understanding, as well as advanced architectures and practical pipelines for real-world segmentation tasks.