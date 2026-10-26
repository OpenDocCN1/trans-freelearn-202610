![](bbox=[0.12, 0.092, 0.847, 0.499])

Figure 6.1: TensorBoard and the SCALARS tab for training and evaluation logs

The logging step is done every 20 iterations during training (this number is configurable), and, by default, there are 12 categories of scalars:

+   - The `data_time` category stores the time it has taken to prepare the data for training.
+   - The `eta_seconds` category stores the estimated number of seconds until training completes.
+   - The `fast_rcnn` category stores the Fast R-CNN evaluation metrics. These are classification accuracy (`cls_accuracy`), false negative (`false_negative`), and foreground (object) classification accuracy (`fg_cls_accuracy`).
+   - The `loss_box_reg` category stores the loss for box regression (or the localization loss).
+   - The `loss_cls` category stores the classification loss.
+   - The `loss_rpn_cls` category stores the region proposal network objectness classification loss.
+   - The `loss_rpn_loc` category stores the region proposal network localization (bounding-box) loss.
+   - The `lr` category stores the learning rate. Learning rates may vary due to having a warm-up period, steps (reduced at some steps), or using other parameters.