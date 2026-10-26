# Training custom D2Go models

Let us take the brain tumor dataset and train a D2Go model to perform brain tumor detection and classification tasks. Similar to the previous section, we need to install the detectron2, mobile-vision, and d2go packages. We must also copy the configs folder to the d2go installation destination and change the line for importing fqn_to_module. These steps remain the same as described in the previous section.

We also need to download the brain tumor dataset and register the train and test datasets to Detectron2 as we have often done while training Detectron2 models (the code remains the same). After downloading the datasets and registering them, let us select a D2Go model for training. For this task, D2Go provides a class called GeneralizedRCNNRunner to help create a configuration file and train custom models. Let us start by importing this class, selecting a model, and creating an instance of this runner class:

```
from d2go.model_zoo import model_zoo
from d2go.runner import GeneralizedRCNNRunner
selected_model = "faster_rcnn_fbnetsv3a_C4.yaml"
nc = 2
output_dir = './output'
model_url = selected_model
runner = GeneralizedRCNNRunner()
```

Now, we are ready to get a configuration for this model before starting training:

```
cfg = runner.get_default_cfg()
cfg.merge_from_file(model_zoo.get_config_file(model_url))
cfg.MODEL.EMA.ENABLED = False
cfg.DATASETS.TRAIN = (name_ds_train,)
cfg.DATASETS.TEST = (name_ds_test,)
cfg.DATALOADER.NUM_WORKERS = 2
cfg.MODEL.WEIGHTS = model_zoo.get_checkpoint_url(model_url)
cfg.MODEL.DEVICE ="cpu" if ('CI' in os.environ) else "cuda"
cfg.SOLVER.IMS_PER_BATCH = 2
cfg.SOLVER.BASE_LR = 0.00025
cfg.SOLVER.MAX_ITER = 5 if ('CI' in os.environ) else 5000
cfg.SOLVER.STEPS = []
cfg.MODEL.ROI_HEADS.BATCH_SIZE_PER_IMAGE = 128
cfg.MODEL.ROI_HEADS.NUM_CLASSES = nc
cfg.OUTPUT_DIR = output_dir
os.makedirs(cfg.OUTPUT_DIR, exist_ok=True)
```