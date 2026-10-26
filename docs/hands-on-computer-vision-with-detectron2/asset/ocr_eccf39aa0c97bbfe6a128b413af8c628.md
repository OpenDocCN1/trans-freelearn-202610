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

# The architecture of the segmentation models

Chapter 4 dived deep into the architecture of Faster R-CNN implemented in Detectron2 for object detection tasks in computer vision. Detectron2 implements Mask R-CNN for object segmentation tasks. It has the same components as Faster R-CNN architecture and one other head for the segmentation task. Figure 10.1 shows the Faster R-CNN architecture as listed in Chapter 4.

![](bbox=[0.12, 0.79, 0.88, 0.88])

Figure 10.1:The architecture of the Detectron2 object detection application