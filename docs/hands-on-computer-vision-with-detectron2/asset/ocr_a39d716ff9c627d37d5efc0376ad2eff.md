Detectron2 provides a function called scripting_with_instances to perform exports using the PyTorch scripting function (torch.jit.script). Detectron2 models use the Instances class, and attributes in this class can be added dynamically with eager mode. The PyTorch scripting function does not support this dynamic behavior. This is why this helper function (scripting_with_instances) is required. It takes the following input parameters:

+   - model: This is a Detectron2 model to be exported using the scripting method
+   - fields: This gives attribute names and types used by the Instances class in the model

This method creates a class called new_Instances, similar to Instances but with static attributes. The static attributes must be declared using the fields input parameter of the helper method. The new_Instances class makes the model traceable. The following is an example usage of this helper function:

```
fields = {
    "proposal_boxes": Boxes,
    "objectness_logits": Tensor,
    "pred_boxes": Boxes,
    "scores": Tensor,
    "pred_classes": Tensor,
    "pred_masks": Tensor,
    "pred_keypoints": Tensor,
    "pred_keypoint_heatmaps": Tensor,
}
model.eval()
scripted_model = scripting_with_instances(model, fields)
```

Congratulations! You now know about the two main utilities that help export the Detectron2 model with tracing and scripting methods. Let us apply these in exporting a custom Detectron2 model built previously.

# Exporting a custom Detectron2 model

We first need to install Detectron2, load a dataset, and get a trained model and its configuration file for exporting:

```
# install
!python -m pip install \
'git+https://github.com/facebookresearch/detectron2.git'
# dataset
```