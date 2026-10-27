

mode = result.data['phase'].mode()[0]
guess = mode / 2**qnum
print(f'target={target:0.4f}, '
f'estimate={guess:0.4f}={mode}/{2**qnum}')
errors.append((target - guess)**2)
rms = np.sqrt(sum(errors) / len(errors))
print(f'RMS Error: {rms:0.4f}\n')

4. 对两个、四个和八个量子比特执行量子相位估计：

```
def main(qnums = (2, 4, 8), repetitions=100):
    for qnum in qnums:
        experiment(qnum, repetitions=repetitions)

if __name__ == '__main__':
    main()
```

前述代码实现了对两个、四个和八个量子比特的量子相位估计。此外，为了获得结果，相位估计电路（experiment）总共运行 100 次重复。

在本节中，我们已经介绍了两种在某些量子算法中起关键作用的量子技术。这些技术是 QFT 和量子相位估计。在下一节中，我们将介绍另一种已被证明比其经典对应方法更强大的量子算法。该算法是 Simon 算法。

## 介绍 Simon 算法

Simon's algorithm 建立在 Bernstein-Vazirani 算法的基础上，该算法已在本章前面讨论过。对于 Simon 算法，假设对于 n 个量子比特，我们有以下函数：

f: {0,1}^n → {0,1}^n

这伴随着一个承诺，即对于某个非零比特串 s，使得：

s ∈ {0,1}^n

并且对于所有 x、y 值，使得：

x, y ∈ {0,1}^n

则：

```
$$f(x) = f(y)$$
```

当且仅当：

```
$$x \oplus y \in \{0^n, s\}$$
```

Simon's algorithm 可以总结如下。首先，使用两个寄存器，每个寄存器有 n 个量子比特。因此，量子系统的初始状态如下所示：

```
$$|\psi_0\rangle = |0 \otimes \cdots \otimes 0\rangle \otimes |0 \otimes \cdots \otimes 0\rangle$$
```

此步骤随后是对第一寄存器中每个量子比特应用 Hadamard 门。这导致以下量子状态：

```
$$|\psi_1\rangle = H|0 \otimes \cdots \otimes 0\rangle \otimes (|0 \otimes \cdots \otimes 0\rangle)$$
```

Simon's algorithm 实现中的下一步是应用酉变换 F（oracle 函数），使得系统的新的状态变为以下：

```
$$|\psi_2\rangle = \frac{1}{\sqrt{2^n}} \sum_{x \in \{0,1\}^n} |x\rangle \otimes |f(x)\rangle$$
```

此外，我们测量第二个寄存器。此步骤随后是对第一寄存器上的所有量子比特应用 Hadamard 门，然后测量这些量子比特。

> **注意**
> 此代码取自 https://qiskit.org/textbook/ch-algorithms/simon.html。
> 
> 此代码的链接可以在这里找到：https://github.com/PacktPublishing/Hands-On-Quantum-Information-Processing-with-Python/tree/master/Chapter05。

正如前面所说，Shor’s algorithm 高效地找到周期函数的周期。周期函数 f(x) 给出如下：

```
f(x) = a^x mod N
```

这里，mod 是模运算符，a 和 N 是没有共同因子的正整数（因此 a 和 N 互质），并且以下适用：

```
a < N
```

此外，周期 r 是使以下条件成立的最小非零整数：

```
a^r mod N = 1
```

Shor’s algorithm 的实现可以总结如下：

- 1. 首先，使用两个寄存器。
- 第一个寄存器由 m 个量子比特组成，而第二个寄存器由 n 个量子比特组成，其中以下适用：

```
m = 2n
```

实现 Shor’s algorithm 的电路如下图所示：

![](img/2508606813a9204c61e8132ec0695cc1_141_0.png)

图 5.9 – 实现 n 量子比特 Shor 算法的量子电路

Shor 算法的两个寄存器都初始化为所有量子比特的 |0> 状态。因此，初始状态如下：

```
ψ₀⟩ = |0⟩ₘ ⊗ |0⟩ₙ
```

- 2. 此步骤随后是对第一个寄存器中的 m 个量子比特中的每一个应用 Hadamard 门。量子系统的状态然后变为以下：

```
ψ₁⟩ = H|0⟩ ⊗ ⋯ ⊗ |0⟩ ⊗ |0⟩ₙ
```

3. 在对第一个寄存器中的量子比特应用 Hadamard 门之后，Shor 算法中的下一步是应用酉变换 F，将系统的状态变换为以下：

$$\psi_2\rangle = \frac{1}{\sqrt{2^m}} \sum_{x\in\{0,1\}^m} |x\rangle \otimes |a^x \bmod N\rangle$$

4. 此步骤随后是对顶部寄存器应用逆 QFT。这随后是对第二个寄存器中的量子比特进行测量。最后，第一寄存器中的量子比特也被测量。

> **注意**
> 此代码的链接可以在这里找到：https://github.com/PacktPublishing/Hands-On-Quantum-Information-Processing-with-Python/tree/master/Chapter05。

使用 Qiskit 实现 Shor 算法的 Python 代码如下所示：

- 1. 第一步涉及导入实现 Shor 算法所需的模块：

```
from qiskit import BasicAer
from qiskit.aqua import QuantumInstance
from qiskit.aqua.algorithms import Shor
```

- 2. 下一步涉及使用 qasm_simulator 实际实现 Shor 算法并对其进行模拟：

```
number = Shor(N=15, a=7)
simulator = BasicAer.get_backend('qasm_simulator')
results_dictionary = number.run(QuantumInstance(
                                    backend=simulator,
                                    shots=5))
#result = results_dictionary['number']
print(results_dictionary)
```

前述代码可以总结如下。

在导入必要的模块和类之后，使用 Qiskit 的 Aqua 元素中的 Shor 类构建用于计算 Shor 算法的电路。

3. 在对元素和 oracle 进行编码之后，Grover 算法实现的下一步是初始化 n 个量子比特。因此，系统状态 |s> 在此阶段将如下所示：

```
$\psi_0\rangle = 0 \otimes \cdots \otimes 0\rangle$
```

4. 下一步是对所有 n 个量子比特应用 Hadamard 门，使得 |s> 变为以下：

```
$\psi_1\rangle = H0 \otimes \cdots \otimes 0\rangle$
```

5. 此步骤随后是对状态应用 F1。下一步是应用另一个变换 F2。对于状态 |s>，该变换如下所示：

```
$F_2(s) = 2s\langle s - I$
```

这里，I 是单位矩阵。因此，量子系统的新的状态将如下所示：

```
$\psi_2\rangle = F_2 F_1 \psi_1\rangle$
```

6. 上一步重复 t 次，使得最终量子系统的状态变为以下：

```
$\psi_t\rangle = (F_2 F_1)^t \psi_1\rangle$
```

7. 最后，对 n 个量子比特执行测量。

> 注意
> 此代码的链接可以在这里找到：https://github.com/PacktPublishing/Hands-On-Quantum-Information-Processing-with-Python/tree/master/Chapter05。

使用 Qiskit 实现 Grover 算法的 Python 代码如下所示：

- 1. 第一步涉及导入实现 Grover 算法所需的模块：

```
from qiskit import QuantumRegister, ClassicalRegister, \
QuantumCircuit, execute, BasicAer
from qiskit.visualization import plot_histogram
import matplotlib.pyplot as plt
```

2. 下一步是定义量子电路以实现 Grover 算法。在这种情况下，要使用的电路包括一个两量子比特量子寄存器和一个两比特经典寄存器：

```
c = ClassicalRegister(2, 'c')
q = QuantumRegister(2,'q')
qc = QuantumCircuit(q,c)
```

3. 然后，实现量子门以实现该电路：

```
qc.h([q[0]])
qc.h([q[1]])
qc.x([q[0]])
qc.x([q[1]])
qc.cz(0,1)
qc.x([q[0]])
qc.x([q[1]])
qc.h([q[0]])
qc.h([q[1]])
qc.z(q[0])
qc.z(q[1])
qc.cz(0,1)
qc.h([q[0]])
qc.h([q[1]])
```
