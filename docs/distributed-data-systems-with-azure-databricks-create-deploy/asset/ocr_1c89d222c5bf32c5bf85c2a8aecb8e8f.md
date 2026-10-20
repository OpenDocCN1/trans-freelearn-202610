The ring allreduce algorithm works in two steps. First, each worker exchanges data so that each worker has a chink of the result, and then runs an allgather operation so that all the workers have a complete result.

Another point where we can improve the communication between workers is by making use of the inherent calculation process of the gradients in a neural network.

To train a neural network, we first run a forward propagation and then a backward propagation to compute the gradients at each of the layers of the neural network, starting from the output layer up to the input layer. Therefore, the first available gradients are the ones from the output layer. We can exploit this process by sending the gradients as soon as they are ready instead of waiting for all of them to be finished, in order to avoid spikes in bandwidth use and overlays in communication with the rest of the workers. This way, we overcome the inconvenience of having to wait for each worker to finish the gradient computation over all of the layers of the neural network.

The good thing about this approach is that it is agnostic from the network architecture and framework used to create the model, and it allows us to better harness the power of parallel computing for distributed training of deep learning models.

The horovod library applies the ring allreduce algorithm to enable distributed training of deep learning networks in distributed computing systems such as Azure Databricks. In the next sections, we will learn how we can leverage the use of HorovodRunner to efficiently train deep learning models.

# Using the Horovod distributed learning library in Azure Databricks

horovod is a library for distributed deep learning training. It supports commonly used frameworks such as TensorFlow, Keras, and PyTorch. As mentioned before, it is based on the tensorflow-allreduce library and implements the ring allreduce algorithm in order to ease the migration from single-graphics processing unit (GPU) training to parallel-GPU distributed training.

In order to do this, we adapt a single-GPU training script of a deep learning model to use the horovod library during the training process. Once we have adapted the script, it can run on single or multiple GPUs without changes to the code.

The horovod library uses a data parallelization strategy by allowing efficient distribution of the training to multiple GPUs in parallel in an optimized way, by implementing the ring allreduce algorithm to overcome communication limitations.