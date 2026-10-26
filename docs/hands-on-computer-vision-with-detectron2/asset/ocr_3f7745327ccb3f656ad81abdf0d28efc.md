```
'model_0002999.pth',
'model_0003499.pth',
'model_0003999.pth',
'model_0004499.pth',
'model_final.pth']
```

This code snippet lists all the models saved at every checkpoint period (cfg.SOLVER.CHECKPOINT_PERIOD) over the training process. Notably, the model after the training completes (at the cfg.SOLVER.MAX_ITER iteration) is named model_final.pth. Additionally, the number in the model name is always padded with zeros to have a length of 7.

The following code snippet prepares an evaluator to evaluate how each of these models performs on the test set:

```
from detectron2.engine import DefaultPredictor
from detectron2.evaluation import COCOEvaluator
evaluator = COCOEvaluator(
    dataset_name = name_ds_test,
    tasks=("bbox",),
    distributed=False,
    output_dir = os.path.join(output_dir, 'test_results')
)
```

Notably, COCOEvaluator is selected because the dataset is in COCO format. Additionally, the "bbox" input for the tasks option indicates to the evaluator to perform evaluations for object detection tasks. The test results are stored in the 'test_results' folder under the output directory. After getting the evaluator, the following code snippet performs the evaluations on each of the generated models:

```
# evaluate all folders
import logging
logger.setLevel(logging.CRITICAL)
model_eval_results = []
for model_name in model_names:
    # load weights
    cfg.MODEL.WEIGHTS = os.path.join(cfg.OUTPUT_DIR, model_name)
    cfg.MODEL.ROI_HEADS.SCORE_THRESH_TEST = 0.5
    # predictor
    predictor = DefaultPredictor(cfg)
    # evaluate
    x = trainer.test(cfg, predictor.model,
```