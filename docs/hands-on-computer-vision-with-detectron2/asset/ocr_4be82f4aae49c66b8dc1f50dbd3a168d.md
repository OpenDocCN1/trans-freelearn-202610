At this point, if all these data processing steps are performed on Google Colab, it is a good time to download the resulting COCO datasets to your local computer or to map the Google Drive folder and save the resulting datasets into a Google Drive folder to avoid losing the datasets after the Google Colab instance is turned off. In this specific case, the converted COCO datasets are stored in the GitHub folder of this book for future sections.

Now that the datasets are ready and correctly registered, it is time to train models and see how they perform using Detectron2. The simplest way to perform model training on custom datasets is to use the default trainer class provided by Detectron2.

# Using the default trainer

Detectron2 provides a default trainer class, which helps to train Detectron2 models on custom datasets conveniently. First, we download the datasets converted in the previous section and unzip them:

```
!wget -q https://github.com/PacktPublishing/Hands-On-Computer-Vision-with-Detectron2/raw/main/datasets/braintumors_coco.zip
!unzip -q braintumors_coco.zip
```

Next, install Detectron2 and register the train/test datasets using the exact code snippets provided in the previous section. Additionally, before training, run the following code snippet to prepare a logger that Detectron2 uses to log training/inferencing information:

```
from detectron2.utils.logger import setup_logger
logger = setup_logger()
```

After having the datasets registered and setting up the logger, the next step is getting a training configuration. Precisely, we set the output directory (where we will store the logging events and the trained models), the path to store the configuration file to be used in the future inferences, the number of classes as positive and negative (nc = 2), and the device option as cuda (for training on GPUs). Finally, to speed up training, we also select the ResNet50 FPN as the backbone network for the model to be trained:

```
import os
from detectron2.config import get_cfg
from detectron2 import model_zoo
import pickle

output_dir = "output/object_detector"
os.makedirs(output_dir, exist_ok=True)
output_cfg_path = os.path.join(output_dir, "cfg.pickle")
nc = 2
```