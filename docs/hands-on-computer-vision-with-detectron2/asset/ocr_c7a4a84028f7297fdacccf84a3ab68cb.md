If this error occurs, you can perform the following steps as a quick fix for this issue:

+   1. Locate the `fsdp.py` file from the d2go installation directory (e.g., on Google Colab, it is `/usr/local/lib/python3.9/dist-packages/d2go/trainer/fsdp.py`).
+   2. Locate and change the path to `fqn_to_module` on line 16 from `'from torch.ao.pruning import fqn_to_module'` to `'from torch.ao.sparsity.sparsifier.utils import fqn_to_module'`.
+   3. Press Ctrl + S keys to save the change.

After completing these quick fixes, we are ready to select an existing D2Go model on its Model Zoo. The ways to read and choose pre-trained models for D2Go are the same as described in Chapter 2 for selecting pre-trained models for Detectron2. Let us select a simple model to demonstrate the steps to load an existing model on the D2Go Model Zoo:

```
from d2go.model_zoo import model_zoo
selected_model = 'faster_rcnn_fgnetv3a_C4.yaml'
model = model_zoo.get(selected_model, trained=True)
```

After this step, we should have the model loaded (model). Let us prepare a simple input and test the loaded model:

```
!wget -qq <url_to/800x800image/input.jpeg>
```

The following code snippet loads the downloaded image before performing inferencing on it:

```
import cv2
img = cv2.imread("./input.jpeg")
```

Like the DefaultPredictor class in Detectron2, D2Go also provides a class called DemoPredictor to generate a predictor from a loaded model. The following code snippet creates an instance of this class given the input model, performs inferencing on the loaded image (img), and prints the results:

```
from d2go.utils.demo_predictor import DemoPredictor
predictor = DemoPredictor(model)
outputs = predictor(img)
pred_instances = outputs["instances"]
print(pred_instances)
```