To make use of the checkpoint feature in Flair, we need to pass the checkpoint=True parameter value to the trainer.train() method when training. But the feature does come at a cost. Writing checkpoints essentially means writing model weights to the disk after each epoch, and that takes time. This means that using checkpoints is likely to slow down the model training process a bit. However, when doing serious model training for longer periods of time, this overhead is almost always worth it.

Assuming our base_dir is tagger and we have a corpus object-loaded memory, we can simply resume training from the last successful epoch by running the following:

```
from flair.trainers import ModelTrainer

chkpoint_path = 'tagger/checkpoint.pt'
trainer = ModelTrainer.load_checkpoint(chkpoint_path, corpus)

trainer.train(...)
```

Here, the arguments passed to trainer.train() should be no different from the parameters we used for the initial training session that got interrupted, where we also used the checkpoint=True model trainer parameter value.

The preceding exercise and explanations should help us train a tagger for just about any language included in Flair's corpora set. But it's entirely possible that the parameters we choose will not yield good results. There are many clues in the model training output we can look out for that will tell us whether training is progressing well or whether we should restart with a different approach.

# Knowing when to stop and try again

The question "Is my model good enough?" is a whole subfield of machine learning on its own and can't fully be explained in a single chapter. Nevertheless, there's still some practical advice we all can follow to help us determine whether it's worth restarting training with different parameters, different model types, or even new data.

The two basic techniques for measuring success during and after training are as follows:

+   1. Monitoring loss
+   2. Assessing and comparing performance metrics