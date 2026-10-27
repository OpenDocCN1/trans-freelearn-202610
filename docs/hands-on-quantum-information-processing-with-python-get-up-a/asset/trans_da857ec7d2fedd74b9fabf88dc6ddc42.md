
2. 随后加载用于实现 ANN 算法的数据，然后将数据集划分为训练集和测试集。20% 的数据用于测试算法的准确性，80% 的数据用于训练模型：

```
iris = load_digits()
X = iris.data
Y = iris.target

x_train, x_test, y_train, y_test = \
train_test_split(X,Y, test_size=0.2, random_state=42)
```

3. 下一步是实际实现 MLP ANN 算法：

```
clf = MLPClassifier(alpha=1, max_iter=1000)
clf.fit(x_train, y_train)
y_pred = clf.predict(x_test)
```

4. 最后，探究 MLP ANN 算法的准确性，如以下代码片段所示：

```
print('Accuracy is:', accuracy_score(y_test, y_pred))
print('\nClassification Report is:\n',
    classification_report(y_test, y_pred))
print('\nConfusion Matrix is:\n',
    confusion_matrix(y_test, y_pred))
```

前面的代码可以总结如下：

1. 第一步是导入将要使用的所有包，然后导入将要使用的数据集。在本例中，要使用的数据集是 MNIST 手写数字数据集，这是机器学习中一个非常流行的数据集。
2. 导入数据集后，进行预处理，然后使用 MLP 分类器对数字进行分类。
3. 最后，计算不同的性能指标。这些指标包括准确性和混淆矩阵。

既然我们已经对 ANN 做了基本介绍，下一步是介绍另一种机器学习算法，即支持向量机（SVM）。

## 探索 SVM

SVM 适用于复杂但小型或中型的数据集。它们利用最大间隔分离器的特性来线性分离数据点。如果无法进行线性分离，则将数据点投影到更高维度，在那里可以实现线性可分性。这种将数据点投影到更高维度的操作是通过核函数完成的。

SVM 可以用于以下任务：

-   线性分类和非线性分类
-   回归
-   异常检测

使用 sklearn 实现 SVM 的 Python 代码片段如下：

> **注意**
> 本章的 GitHub 链接可以在以下位置找到：
> https://github.com/PacktPublishing/Hands-On-Quantum-Information-Processing-with-Python/tree/master/Chapter08

1. 此代码片段中的第一步是导入必要的模块：

```
import numpy as np
from sklearn.datasets import load_digits
from sklearn.model_selection import train_test_split
from sklearn.svm import SVC
from sklearn.metrics import accuracy_score, classification_report, confusion_matrix

np.random.seed(42)
```

2. 随后加载用于 SVM 分类算法的数据，并将数据划分为训练集和测试集。在本例中，20% 的数据将用于测试算法，80% 的数据将用于训练 SVM 算法：

```
iris = load_digits()
X = iris.data
```

通常，根据所使用的数据集类型（经典或量子）以及所使用的计算平台/算法（经典或量子），机器学习可以总结为下图：

| 数据类型 | 经典 | 量子 |
| --- | --- | --- |
| 经典 | CC | CQ |
| 量子 | QC | QQ |

图8.2 – 机器学习方法总结

在上图中，CC 方法表示在经典硬件上使用经典数据集，而 QC 方法表示在经典计算机上使用量子数据集。另一方面，CQ 方法表示在量子计算机上使用经典数据集，而 QQ 方法表示在量子计算机上使用量子数据集。

CQ 和 QQ 方法都可以视为量子机器学习。本章中，我们将重点关注 CQ 方法——在量子计算机上使用经典数据集。

到目前为止，已经开发出多种量子机器学习算法，包括：

-   量子神经网络
-   量子主成分分析
-   量子 SVM
-   变分量子机器学习算法，如**变分量子本征求解器（VQE）**和**量子近似优化算法（QAOA）**

到目前为止，我们已经对量子机器学习做了简要介绍。我们还看到，在 CQ 方法中，可以在量子计算机上使用经典数据集。接下来，我们将讨论如何对经典数据进行编码，以便量子计算机能够处理。

## 数据编码

有多种方法可以对经典数据进行编码，以便在量子计算机上进行处理。本节简要讨论这些数据编码技术：基编码、振幅编码和哈密顿量编码。

### 基编码

基编码将数据编码到量子计算机的基本（计算）态中。也就是说，每个数据点表示为基本态的叠加。这种编码形式仅限于二进制数据。

### 振幅编码

另一方面，振幅编码将数据编码到量子态的振幅上。与基编码不同，振幅编码不限于二进制数据。然而，振幅编码容易受到噪声的影响，因为信息编码在振幅上，而振幅可能随噪声变化。

### 哈密顿量编码

最后，哈密顿量编码使用系统的哈密顿量来编码数据。也就是说，在这种数据编码方案中，系统的哈密顿量与表示数据的厄米矩阵相关联。

我们简要介绍了一些数据编码技术。下一小节将介绍量子 SVM。

## 量子 SVM

量子 SVM 是一类重要的量子机器学习算法。本质上，量子 SVM 是前一节中介绍的传统 SVM 的量子对应物。量子计算机可以加速 SVM 的学习过程。

接下来展示的 Python 代码片段说明了如何使用 qiskit 实现量子 SVM：

> **注意**
> 本章的 GitHub 链接可以在以下位置找到：
> https://github.com/PacktPublishing/Hands-On-Quantum-Information-Processing-with-Python/tree/master/Chapter08

1. 第一步涉及导入实现量子 SVM 算法所需的模块：

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

2. 随后加载数据集并准备数据（预处理），用于实现量子 SVM：

```
feature_dim = 4
_, training_input, test_input, _ = breast_cancer(
    training_size=12,
    test_size=4,
    n=feature_dim)

feature_map = ZZFeatureMap(feature_dimension=feature_dim,
```

## 量子变分分类器

另一类重要的量子机器学习算法是量子变分算法。本质上，量子变分算法是经典-量子混合学习算法。

下面展示的 Python 代码片段说明了如何使用 qiskit 实现量子变分分类器：

> **注意**
> 本章的 GitHub 链接可以在以下位置找到：
> https://github.com/PacktPublishing/Hands-On-Quantum-Information-Processing-with-Python/tree/master/Chapter08

1. 实现量子变分分类器的第一步是导入必要的模块：

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

2. 下一步是准备用于实现量子变分分类器算法的数据：

```
feature_dim = 4 # dimension of each data point
_, training_input, test_input, _ = breast_cancer(
    training_size=12,
    test_size=4,
    n=feature_dim)
```

