# Performing a computer vision task

After having all the corresponding methods for getting a predictor (get_predictor), performing inference (perform_inference), and visualizing output (visualize_output), we are ready to perform detections. Here is the code snippet for performing object detection tasks using these methods:

```
config_file = "COCO-Detection/faster_rcnn_X_101_32x8d_FPN_3x.yaml"
checkpoint_url = "COCO-Detection/faster_rcnn_X_101_32x8d_FPN_3x.yaml"
score_thresh_test = 0.95
input_url = "input.jpeg"
task = OBJECT_DETECTION
scale = 0.5
# get a predictor
cfg, predictor = get_predictor(config_file, checkpoint_url, score_thresh_test)
# perform inference
img, output = perform_inference(predictor, input_url)
# visualization
visualize_output(img, output, cfg, task, scale)
```

Other computer vision tasks can be performed in the same way. Furthermore, these methods can be placed into a library (a *.py file), so you can include them and use them in any computer vision applications if appropriate.

# Summary

This chapter discussed the pre-trained models available on the Detectron2 Model Zoo. Specifically, it specified the vital information to look for while selecting a pre-trained model for a computer vision task. It then provided the code snippets for developing an object detection application, an object instance segmentation application, a keypoint detection application, a panoptic segmentation application, and a semantic segmentation application. The code snippets for these applications are similar. Therefore, the code snippets are abstracted into typical methods for getting a model, performing inferences, and visualizing outputs. These methods can then be reused to develop different computer vision applications with Detectron2 quickly.