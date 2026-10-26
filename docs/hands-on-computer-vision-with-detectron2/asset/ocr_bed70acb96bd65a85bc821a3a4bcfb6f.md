# Introduction to PointRend

PointRend is a project as a part of Detectron2. It helps provide better segmentation quality for detected object boundaries. It can be used with instance segmentation and semantic segmentation. It is an extension of the Mask R-CNN head, which we discussed in the previous chapter. It performs point sampling on the detected mask and performs predictions on the sampled points instead of all the points in the mask. This technique allows us to compute the mask with a higher resolution, thus providing a higher mask resolution. Figure 11.1 illustrates an example of two images when not using (left) and when using (right) PointRend:

![](bbox=[0.19, 0.29, 0.48, 0.57])

![](bbox=[0.51, 0.29, 0.81, 0.57])

Figure 11.1: Segmentation quality with and without PointRend

PointRend helps render segmentations that are of a higher resolution with object boundaries that are crisp and less smooth. Therefore, it is useful if the objects to detect have sharp edges.

At inference time, starting from the coarse prediction, it performs upsampling via several steps to increase the mask resolution. At each step, there are three main tasks:

+   1. Sample a number (hyperparameter) of points for performing predictions. This strategy helps avoid produced computing predictions on many grid points at higher resolutions.
+   2. Extract features for each sampled point from the feature maps extracted by the backbone, combined with the features from the coarse prediction.
+   3. Classify each sampled point's labels using their extracted features using a multilayer perceptron neural network (Point Head).