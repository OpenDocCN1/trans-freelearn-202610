Once we have defined the function that will handle the model creation, compilation, and training, we can run the training using the Spark TensorFlow Distributor MirroredStrategyRunner class in local mode. This mode will train the model in all the driver-available GPUs. The code is shown in the following snippet:

```
from spark_tensorflow_distributor import MirroredStrategyRunner
runner = MirroredStrategyRunner(num_slots=1,
                                local_mode=True,
                                use_gpu=USE_GPU)
runner.run(train)
```

In this very simple way, we can execute the code in the local node by setting the local_mode parameter to True. Now, in order to run the distributed training, we will set the local_mode parameter to False as we want our function to run on the workers of the Spark cluster.

We will define a number of slots that depend on whether we have a GPU available or not. If we have a GPU available, the number of slots will be set to the number of available GPUs. If we are using a CPU cluster, we should set the number of slots to a reasonable number, which in our case is 4, as can be seen in the following code snippet:

```
NUM_SLOTS = CLUSTER_GPUS if USE_GPU else 4
runner = MirroredStrategyRunner(num_slots=CLUSTER_GPUS,
                                use_gpu=USE_GPU)
runner.run(train)
```

The MirroredStrategyRunner class is a strategy designed for nodes that have multiple GPUs available, but we can also specify a custom strategy for the distributed training. To do so, you need to create a tf.distribute.Strategy object within the train() function and set use_custom_strategy to True in the MirroredStrategyRunner function.

As an example, we can change the distributed training strategy to make use of tf.distribute.experimental.MultiWorkerMirroredStrategy. The MultiWorkerMirroredStrategy strategy is designed to distribute the training across workers that may or may not have GPUs in a synchronous manner. It is similar to MirroredStrategy but differs in the fact that it implements an All-Reduce execution mode to be able to make the nodes work together. The code is shown here:

```
def train_custom_strategy():
    import tensorflow as tf
```