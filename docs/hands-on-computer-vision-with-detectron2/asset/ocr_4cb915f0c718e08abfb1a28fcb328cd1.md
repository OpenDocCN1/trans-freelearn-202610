```
from detectron2 import model_zoo
from detectron2.engine import DefaultPredictor
# We suppress some user warnings
import warnings
warnings.simplefilter(action='ignore',category=UserWarning)
# Create a configuration and a predictor
def get_predictor(config_file, checkpoint_url, score_thresh_test=1.0):
    # create a configuration object
    cfg = get_cfg()
    # get the configurations from the config_file
    config_file = model_zoo.get_config_file(config_file)
    cfg.merge_from_file(config_file)
    # get the pre-built weights of a trained model from the checkpoint
    cfg.MODEL.WEIGHTS = model_zoo.get_checkpoint_url(checkpoint_url)
    # set the threshold for recall vs. precision at test time
    cfg.MODEL.ROI_HEADS.SCORE_THRESH_TEST = score_thresh_test
    # create a predictor
    predictor = DefaultPredictor(cfg)
    return cfg, predictor
```

To get a model, you only need to select the paths to the configuration file and pre-trained weights and pass them to this method. This method then returns a predictor (predictor) together with this model's configuration (cfg). After having the predictor, the next step is to perform inferences.

# Performing inferences

The code snippet for all the computer vision tasks is precisely the same. Therefore, we should provide a method for this (perform_inference). Here is the code snippet for performing inference given any predictor (predictor) and the path to the input image (input_url):

```
import cv2
from google.colab.patches import cv2_imshow
def perform_inference(predictor, input_url):
    img = cv2.imread(input_url)
    output = predictor(img)
    return img, output
```