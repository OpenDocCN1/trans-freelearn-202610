
We use the tumor type (category_id) as a stratified field to split tumor types between the train and test sets. The following code snippet helps to create train and test directories, move the images, and create the corresponding annotation files.

First, we create the corresponding folders for the train and test datasets:

```
train_path = os.path.join(name_ds, "train")
test_path = os.path.join(name_ds, "test")
train_af = os.path.join(train_path, af)
test_af = os.path.join(test_path, af)
os.makedirs(train_path, exist_ok=True)
os.makedirs(test_path, exist_ok=True)
```

Next, we move images to their corresponding train and test folders:

```
for img in tqdm(images_train):
    frm = os.path.join(name_ds, img["file_name"])
    to = train_path
    shutil.move(frm, to)
for img in tqdm(images_test):
    frm = os.path.join(name_ds, img["file_name"])
    to = test_path
    shutil.move(frm, to)
```

Finally, we write annotations to the corresponding train and test folders:

```
create_json(info,
            licenses,
            categories,
            images_train,
            annotations_train,
            file_name = train_af)
create_json(info,
            licenses,
            categories,
            images_test,
            annotations_test,
            file_name = test_af)
```

Optionally, you may want to run the following code snippet to remove the current annotation file of the whole dataset:

```
!rm {name_ds}/{af}
```

If you are running this notebook on Google Colab, it is time to zip this dataset before downloading it using the following snippet:

```
!zip -q -r {name_ds}.zip {name_ds}
```

Lastly, the following code snippet downloads this dataset to the local computer for future use:

```
from google.colab import files
files.download(f"{name_ds}.zip")
```

Congratulations! By this time, you should have mastered the components of the COCO annotation format and how to perform the necessary steps to extract data from various sources and form datasets in this annotation format from scratch. We are ready to train a custom segmentation application in Detectron2. However, we should first understand the architecture of such an application before training one.

## The architecture of the segmentation models

Chapter 4 dived deep into the architecture of Faster R-CNN implemented in Detectron2 for object detection tasks in computer vision. Detectron2 implements Mask R-CNN for object segmentation tasks. It has the same components as Faster R-CNN architecture and one other head for the segmentation task. Figure 10.1 shows the Faster R-CNN architecture as listed in Chapter 4.

![](img/0e7994c850a84b7a53d34c7f943c7edd_239_0.png)

Figure 10.1:The architecture of the Detectron2 object detection application

The code to explore the architecture of Detectron2 implementation for object segmentation tasks remains the same as that for the object detection tasks. Therefore, this section only covers some discrepancies between the two. The first difference is that we must select a pre-trained model with configuration for the object segmentation task instead of the detection task. Therefore, we change the corresponding paths for the configuration and model weights as follows:

```
config_file = "<url_to/mask_rcnn_R_50_C4_1x.yaml>"
checkpoint_url = "<url_to/mask_rcnn_R_50_C4_1x.yaml>"
```

Once again, as discussed in *Chapter 2*, other models are available, and there are trade-offs between selecting one versus the others. This selection selects this model (Res50C4) due to its simplicity. The backbone and region proposal networks remain the same and work similarly for object detection tasks. The Region of Interest Heads (`roi_heads`) has one additional head called the mask head (`mask_head`). Therefore, the output of the following code snippet reflects the differences:

```
roi_heads = predictor.model.roi_heads
print(roi_heads)
```

This snippet, besides pooler, res5, and box_predictor as in *Chapter 4*, also displays the mask head as the following:

```
(mask_head): MaskRCNNConvUpsampleHead(
(deconv): ConvTranspose2d(2048, 256, kernel_size=(2, 2),
stride=(2, 2))
(deconv_relu): ReLU()
(predictor): Conv2d(256, 80, kernel_size=(1, 1), stride=(1, 1))
)
```

This mask head is an instance of MaskRCNNUpsampleHead (for this specific case). It is relatively simple, consisting of a ConvTranspose2d layer with ReLU activation and a Conv2d layer. This mask head is configurable via the name parameter in the configuration file (`cfg.MODEL.ROI_MASK_HEAD.NAME`).

Figure 10.2 shows the expanded Region Of Interest Heads with mask_head for object segmentation tasks.

![](img/0e7994c850a84b7a53d34c7f943c7edd_241_0.png)

Figure 10.2: Region of Interest Heads expanded with the mask head for segmentation tasks

Specifically, all the inputs/outputs and intermediate processing are the same as those from the architecture for the object detection tasks (they are faded out). The extension part (not faded) utilizes the predicted bounding boxes from the detection task as its proposals. It then utilizes another Box Features Extraction component that works the same as in the bounding box prediction part. The only difference is that it uses the predicted boxes of the previous step as proposals instead of the original proposals from the region proposal network. Other than that, this component works the same way. The following code snippet shows how this component works:

```
# extract features from the predicted boxes
pred_boxes = [
    instance.pred_boxes
    for instance
    in pred_instances
]
x = roi_heads.pooler([res4], pred_boxes)
x = roi_heads.res5(x)
print(x.shape)
```

Chapter 4 provided details about the pooler and res5 components. This code snippet should display the extracted features as a tensor of the size torch.Size([5, 2048, 7, 7]). In this case, the first value (5) means the number of boxes predicted in the previous step (detection task). Next, the extracted features, together with the predicted instances (outputs of the previous steps), are passed through the mask head:

```
output = mask_head(x, pred_instances)
```

Besides the fields from the instances predicted by the object detection task (pred_boxes, pred_classes, and scores), this output has a field for the predicted masks (pred_masks), one per predicted box. Additionally, if it is at the training stage, this mask head takes the ground-truth segmentations and updates the overall loss by adding the mask loss (mask_loss), as shown in Figure 10.2. The following code snippet displays the sizes of the predicted masks:

```
print(output[0].pred_masks.shape)
```

There is a corresponding predicted mask per predicted box. Therefore, this code snippet should display the tensor size of torch.Size([5, 1, 14, 14]). Again the first number (5) is the number of predicted bounding boxes. The mask resolution is configurable via the following parameter:

```
cfg.MODEL.ROI_MASK_HEAD.POOLER_RESOLUTION
```

This value is currently 14 (as the output mask resolution). The following code snippet visualizes these predicted masks:

```
import matplotlib.pyplot as plt
fig, axes = plt.subplots(1, 5, figsize=(15, 3))
for i in range(5):
    mask = output[0].pred_masks[i][0]
    mask = mask.detach().to("cpu")
    axes[i].imshow(mask)
plt.tight_layout()
plt.show()
```

This code snippet extracts the predicted masks, detaches them, and visualizes them. Figure 10.3 shows the output of this code snippet.

![](img/0e7994c850a84b7a53d34c7f943c7edd_243_0.png)

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
