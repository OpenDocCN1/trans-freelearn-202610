This code snippet has the following output:

```
torch.Size([1, 15, 50, 50])
torch.Size([1, 60, 50, 50])
```

Specifically, there are 1x15x50x50 anchors. Thus, there are 1x15x50x50 values in pred_objectness_logits to indicate whether there is an object in each of the anchors. Similarly, each of the 15 anchors per feature point has 4 values for coordinates (x0, y0, x1, y1), which explains the 60 (15x4) for the output of pred_anchor_deltas. After having the anchors and related predictions, the next step during the training stage is calculating the RPN loss.

# The RPN loss calculation

This loss includes the classification loss from the classification head (predicting objectness scores for anchors) and the regression loss (predicting anchor deltas). Therefore, there must be a label for each of the generated anchors. An anchor is either positive (there is an object, and the label value is 1), negative (there is no object, and the label value is 0), or something else (to be ignored, and the label value is -1). The label assignment task utilizes the measurement of how much an anchor is overlapped with the ground-truth boxes. The popular measurement for this purpose is Intersection over Union (IoU). Figure 4.9 illustrates the computation of IoU.

![](bbox=[0.32, 0.63, 0.7, 0.73])

Figure 4.9: Computation of Intersection over Union (IoU)

Specifically, IoU between two boxes is computed by the ratio of the intersection between them and the union between them.

An anchor has a positive label if it (i) has the highest IoU with a ground-truth box or (ii) has an IoU value that is higher than a specified threshold (e.g., 0.7) with any ground-truth boxes. The second condition should be sufficient, but the first condition helps in cases where there are rare bounding boxes. An anchor has a negative label if all its IoU values with the ground-truth boxes are smaller than a threshold (e.g., 0.3). All other labels are ignored (have a label value of -1). These two IoU thresholds can be customized using Detectron2 configuration parameters. The following code snippet displays these two values in the current model:

```
print(cfg.MODEL.RPN.IOU_THRESHOLDS)
```