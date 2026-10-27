
2. The data to be used for the implementation of the ANN algorithm is then loaded. This is followed by splitting the dataset into the training and the test data. 20% of the data is used to test the accuracy of the algorithm, while 80% of the data is used to train the model:

```
iris = load_digits()
X = iris.data
Y = iris.target

x_train, x_test, y_train, y_test = \
train_test_split(X,Y, test_size=0.2, random_state=42)
```

3. The next step is to actually implement the MLP ANN algorithm:

```
clf = MLPClassifier(alpha=1, max_iter=1000)
clf.fit(x_train, y_train)
y_pred = clf.predict(x_test)
```

4. Finally, the accuracy of the MLP ANN algorithm is explored, as shown in the following code snippet:

```
print('Accuracy is:', accuracy_score(y_test, y_pred))
print('\nClassification Report is:\n',
    classification_report(y_test, y_pred))
print('\nConfusion Matrix is:\n',
    confusion_matrix(y_test, y_pred))
```

The preceding code can be summarized as follows:

1. The first step is to import all the packages that will be used. This is followed by importing the dataset that will be used. In this case, the dataset to be used is the MNIST digits dataset, which is a popular dataset in machine learning.
2. After importing the dataset, then pre-processing is done, followed by classifying the digits with an MLP classifier.
3. Finally, different performance metrics are calculated. These metrics include accuracy and a confusion matrix.

Now that we have provided a basic introduction to ANNs, the next step is to cover another machine learning algorithm, namely SVMs.

## Exploring SVMs

SVMs are suitable for complex but small or medium-sized datasets. They are used to linearly separate data points using the property of maximum margin separator. If the linear separation is not possible, then the data points are projected to higher dimensions where linear separability is possible. This projection of data points to higher dimensions is done using the kernel function.

SVMs can be used in the following tasks:

- Linear and non-linear classification
- Regression
- Anomaly detection

The Python code snippet for implementing an SVM using sklearn is as follows:

> **Note**
> The GitHub link for this chapter can be found here:
> https://github.com/PacktPublishing/Hands-On-Quantum-Information-Processing-with-Python/tree/master/Chapter08

1. The first step in this code snippet is to import the necessary modules:

```
import numpy as np
from sklearn.datasets import load_digits
from sklearn.model_selection import train_test_split
from sklearn.svm import SVC
from sklearn.metrics import accuracy_score, classification_report, confusion_matrix

np.random.seed(42)
```

2. This is followed by loading the data that will be used for the SVM classification algorithm and splitting the data into training and test sets. In this case, 20% of the data will be used to test the algorithm, while 80% will be used to train the SVM algorithm:

```
iris = load_digits()
X = iris.data
```

Generally, depending on the kind of dataset (classical or quantum) being used and the computing platform/algorithm (classical or quantum) used, machine learning can be summarized as shown in the following figure:

| Type of Data | Classical | Quantum |
| --- | --- | --- |
| Classical | CC | CQ |
| Quantum | QC | QQ |

Figure 8.2 – A summary of machine learning approaches

In the preceding diagram, the CC approach denotes the use of a classical dataset on classical hardware, while the QC approach denotes the use of a quantum dataset on a classical computer. On the other hand, the CQ approach denotes the use of a classical dataset on a quantum computer, while the QQ approach denotes the use of a quantum dataset on a quantum computer.

Both the CQ and QQ approaches can be considered as quantum machine learning. In this chapter, our focus will be on the CQ approach – the use of classical datasets on quantum computers.

So far, a variety of quantum machine learning algorithms have been developed. These include the following:

- Quantum neural networks
- Quantum principal component analysis
- Quantum SVMs
- Variational quantum machine learning algorithms such as the **variational quantum eigensolver (VQE)** and **quantum approximate optimization algorithm (QAOA)**

So far, we have provided a brief introduction to quantum machine learning. We have also seen that in the CQ approach, a classical dataset can be used on a quantum computer. Next, we will discuss how classical data can be encoded so that it can be processed by a quantum computer.

## Data encoding

There are various ways of encoding classical data so that it can be processed on a quantum computer. These data encoding techniques are discussed in brief in this section. They are basis encoding, amplitude encoding, and Hamiltonian encoding.

### Basis encoding

In basis encoding, data is encoded in the basic (computational) states of a quantum computer. That is, each data point is represented as the superposition of the basic states. This form of encoding is limited to binary data.

### Amplitude encoding

On the other hand, in amplitude encoding, data is encoded on the amplitudes of a quantum state. Unlike basis encoding, amplitude encoding is not limited to binary data. However, amplitude encoding is vulnerable to noise, as the information is encoded in the amplitude, which might vary with noise.

### Hamiltonian encoding

Finally, in Hamiltonian encoding, data is encoded using the Hamiltonian of the system. That is, in this data encoding scheme, the Hamiltonian of the system is associated with a Hermitian matrix that represents the data.

We have briefly gone through some of the data encoding techniques. The next subsection will cover quantum SVMs.

## Quantum SVMs

Quantum SVMs form a class of prominent quantum machine learning algorithms. In essence, quantum SVMs are the quantum analogs of the conventional SVMs introduced in the previous section. Quantum computers can speed up learning in SVMs.

The python code snippet presented next shows the implementation of the quantum SVM using qiskit:

> **Note**
> The GitHub link for this chapter can be found here:
> https://github.com/PacktPublishing/Hands-On-Quantum-Information-Processing-with-Python/tree/master/Chapter08

1. The first step involves importing the modules required for the implementation of the quantum SVM algorithm:

```
from qiskit import BasicAer
from qiskit.aqua import QuantumInstance, aqua_globals
from qiskit.aqua.algorithms import VQC, QSVM
from qiskit.aqua.components.multiclass_extensions import *
from qiskit.aqua.components.optimizers import COBYLA
from qiskit.aqua.components.feature_maps import RawFeatureVector
from qiskit.circuit.library import ZZFeatureMap, ZFeatureMap, PauliFeatureMap
from qiskit.ml.datasets import breast_cancer
from qiskit.circuit.library import TwoLocal

seed = 42
aqua_globals.random_seed = seed
```

2. This is followed by the loading of the dataset and the preparation of data (pre-processing) for the implementation of the quantum SVM:

```
feature_dim = 4
_, training_input, test_input, _ = breast_cancer(
    training_size=12,
    test_size=4,
    n=feature_dim)

feature_map = ZZFeatureMap(feature_dimension=feature_dim,
```

## Quantum variational classifier

The other prominent class of quantum machine learning algorithms is the quantum variational algorithms. Essentially, quantum variational algorithms are classical-quantum hybrid learning algorithms.

The python code snippet for implementing a quantum variational classifier using qiskit is shown as follows:

> **Note**
> The GitHub link for this chapter can be found here:
> https://github.com/PacktPublishing/Hands-On-Quantum-Information-Processing-with-Python/tree/master/Chapter08

1. The first step in the implementation of the quantum variational classifier is to import the necessary modules:

```
from qiskit import BasicAer
from qiskit.aqua import QuantumInstance, aqua_globals
from qiskit.aqua.algorithms import VQC
from qiskit.aqua.components.optimizers import COBYLA
from qiskit.circuit.library import ZZFeatureMap,\
ZFeatureMap, PauliFeatureMap
from qiskit.aqua.components.feature_maps import RawFeatureVector
from qiskit.ml.datasets import breast_cancer
from qiskit.circuit.library import TwoLocal

seed = 42
aqua_globals.random_seed = seed
```

2. The next step is the preparation of data for the implementation of the quantum variational classifier algorithm:

```
feature_dim = 4 # dimension of each data point
_, training_input, test_input, _ = breast_cancer(
    training_size=12,
    test_size=4,
    n=feature_dim)
```
