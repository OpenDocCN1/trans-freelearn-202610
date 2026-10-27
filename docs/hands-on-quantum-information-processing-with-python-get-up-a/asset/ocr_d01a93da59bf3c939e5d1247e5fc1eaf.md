The operation of the continuous-variable QKD exchange between Alice and Bob can be summarized as follows:

+   1. Alice first randomly selects the position and momentum variables. She then uses these random variables to prepare coherent states.
+   2. Alice then sends the prepared coherent states over to Bob over a noisy channel.
+   3. On the other side, Bob performs either homodyne or heterodyne measurement, choosing position and momentum bases at random.
+   4. Finally, Alice and Bob communicate over a classical channel, to perform classical post-processing.

In this section, we have provided background information on continuous-variable QKD. In the next section, we will explore another field of continuous-variable QIP, namely continuous-variable quantum machine learning.

# Understanding continuous-variable quantum machine learning

In this section, we discuss the continuous-variable version of quantum machine learning. As we have already learned, this approach to machine learning uses continuous variable quantum systems instead of discrete variable quantum systems such as a qubit.

One of the software toolkits that can be used to implement continuous-variable quantum machine learning is the PennyLane framework. This framework supports both discrete-variable quantum machine learning and continuous-variable quantum machine learning. Furthermore, the PennyLane software toolkit, which is based on the Python programming language, also supports classical-quantum hybrid algorithms such as variational algorithms.