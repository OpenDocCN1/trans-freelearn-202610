This code snippet extracts the predicted masks, detaches them, and visualizes them. Figure 10.3 shows the output of this code snippet.

![](bbox=[0.12, 0.165, 0.89, 0.275])

Figure 10.3: The masks (14x14 resolution) of the five predicted instances

These masks have distorted shapes of the detected instances because they have a squared resolution (14x14 in this case), while the predicted instances may have different ratios as in pred_boxes. Therefore, there is an extra step to scale these masks to the corresponding predicted bounding box sizes. The postprocessing step does this scaling:

```
from detectron2.modeling.postprocessing import detector_postprocess
post_output = detector_postprocess(
    results      = output[0],
    output_height = 800,
    output_width  = 800,
)
```

This code snippet takes the output instance and the original input image size (800 by 800 pixels) and scales all the predicted results into sizes corresponding to the image size. Additionally, the predicted masks are scaled to their corresponding bounding box sizes. Finally, the following code snippet visualizes the predicted results:

```
metadata = MetadataCatalog.get(cfg.DATASETS.TRAIN[0])
v = Visualizer(img[:, :, ::-1], metadata, scale=0.5)
instances = post_output.to("cpu")
annotated_img = v.draw_instance_predictions(instances)
imshow(annotated_img.get_image())
```