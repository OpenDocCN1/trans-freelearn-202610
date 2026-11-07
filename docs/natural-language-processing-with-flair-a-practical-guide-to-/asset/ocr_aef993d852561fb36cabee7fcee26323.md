# Training the model

Training the text classifier model involves two simple steps:

+   1. Defining the model trainer class by passing in the classifier model and the corpus
+   2. Setting off the training process passing in the required training hyperparameters

The following classification model training syntax is very similar to how we trained sequence labeling models in the previous chapters:

```
```python
from flair.trainers import ModelTrainer

trainer = ModelTrainer(classifier, corpus)

trainer.train('classifier',
    learning_rate=0.1,
    mini_batch_size=32,
    max_epochs=40)
```
```

The preceding code snippet sets off the training process where we pass in a number of training-related hyperparameters, all of which were explained in detail in Chapter 5, Training Sequence Labeling Models. The training process will now run for 40 epochs or until the model stops improving and will store the final model in the classifier directory.

> **Important Note**
> 
> In the preceding exercise, we used the `DocumentPoolEmbeddings` document embeddings. However, if you ever plan to use `TransformerDocumentEmbeddings` (which is almost guaranteed to offer better performance), you should use the `ModelTrainer.fine_tune()` method instead of `ModelTrainer.train()`, which presets some of the training hyperparameters to aid fine-tuning transformers.

Training should take about 30 minutes on an average CPU. When done, we'll be ready to test the new classifier out.

Upon completion, the model trainer will print out a bunch of metrics. The simplest and most telling metric to look out for is the test_score metric. It shows the classification accuracy of the test dataset. In our case, test_score was 0.672, although be aware that yours will be slightly different due to the randomly initialized weights.