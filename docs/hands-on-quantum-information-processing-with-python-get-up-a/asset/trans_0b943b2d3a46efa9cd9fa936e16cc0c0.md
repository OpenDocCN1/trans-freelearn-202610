

2. 然后，定义实现 CHSH 游戏的量子线路：

```
def CHSH_circuit(x,y):
    a0=0
    a1=np.pi/2
    b0=np.pi/4
    b1=-np.pi/4
    circ = QuantumCircuit(2,2)
    circ.h(0)
    circ.cx(0,1)
    if(x==0):
        circ.ry(a0,0)
    else:
        circ.ry(a1,0)
    if(y==0):
        circ.ry(b0,1)
    else:
        circ.ry(b1,1)
    circ.measure([0,1], [0,1])
    return circ
```

3. 此外，定义 Alice 和 Bob 赢得该游戏的概率：

```
def winning_probability(backend, shots):
    a0=0
    a1=np.pi/2
    b0=np.pi/4
    b1=-np.pi/4
    total = 0
    circuits = [CHSH_circuit(0,0), CHSH_circuit(0,1),
                CHSH_circuit(1,0), CHSH_circuit(1,1)]
    job = execute(circuits, backend=backend,
                  shots = shots)
    for qc in circuits[0:3]:
        counts = job.result().get_counts(qc)
```
```
```python
if ('00' in counts):
    total += counts['00']
if ('11' in counts):
    total += counts['11']
counts = job.result().get_counts(circuits[3])
if ('01' in counts):
    total += counts['01']
if ('10' in counts):
    total += counts['10']
return total/(4*shots)
```
```

4. 最后，使用 'qasm_simulator' 模拟 CHSH 电路并显示获胜概率：

```
```python
backend = Aer.get_backend('qasm_simulator')
print(winning_probability(backend, shots=1024))
```
```

前述代码可以总结如下。首先，导入必要的 Python 模块。然后构建 CHSH 量子电路，使用两个量子位（Alice 和 Bob）。该量子电路首先将两个量子位初始化为量子态 |0>。然后创建 Bell 对。此外，根据 Alice 和 Bob 的经典比特 *x* 和 *y*，Alice 和 Bob 分别应用适当的旋转门。最终，Alice 和 Bob 测量各自的量子位。

在构建实现 CHSH 游戏的量子电路之后，前述 Python 代码中的下一步是使用 'qasm_simulator' 模拟电路。然后显示获得的结果。

在本小节中，我们介绍了 CHSH 量子游戏。我们还提供了实现 CHSH 游戏的 Python 代码。在下一小节中，我们将探索 GHZ 游戏。

## 理解 GHZ 游戏

就像之前讨论的 CHSH 量子游戏一样，**Greenberger-Horne-Zeilinger (GHZ)** 量子游戏也是非局域量子游戏的一个例子。它用于见证量子系统中的三体纠缠。然而，与 CHSH 游戏不同，GHZ 游戏是一个三人游戏（Alice、Bob 和 Charlie 是玩家）。

本质上，GHZ 游戏的设置与 CHSH 游戏相似，只是前者中有三个玩家（Alice、Bob 和 Charlie）最初共享纠缠（三体纠缠），而后者只有两个玩家（Alice 和 Bob）共享纠缠（二体纠缠）。

三人 GHZ 量子游戏可以总结如下。考虑三个玩家，即 Alice、Bob 和 Charlie。裁判将比特 x、y 和 z 分别发送给 Alice、Bob 和 Charlie。作为回应，玩家 Alice、Bob 和 Charlie 分别输出比特 a、b 和 c。玩家获胜的条件为：

a ⊕ b ⊕ c = x ∨ y ∨ z

以下流程总结了 GHZ 量子游戏的实现：

- 在游戏开始之前，确保三个玩家共享三体纠缠。
- 每个玩家在收到 1 时对其量子位应用 Hadamard 门。
- 每个玩家测量其量子位状态。

以下步骤展示了使用 Python 实现 GHZ 量子游戏：

1. 第一步是导入所需的模块：

```
from qiskit import *
from qiskit.visualization import plot_histogram
import numpy as np

np.random.seed(42)
```

2. 下一步是定义用于实现三量子位 GHZ 游戏的量子电路：

```
circ = QuantumCircuit(3,3)
circ.h(0)
circ.cx(0,1)
circ.cx(0,2)
circ.h(0)
circ.h(1)
circ.h(2)
circ.measure([0,1,2], [0,1,2])
```

3. 下一步是使用 'qasm_simulator' 模拟 GHZ 游戏：

```
backend = Aer.get_backend('qasm_simulator')
shots= 1024
job = execute(circ, backend=backend, shots = shots)
result = job.result()
count = result.get_counts(circ)
```

4. 最终，计算并显示赢得 GHZ 游戏的概率：

```
total = 0
if('000' in count):
    total += count['000']
if('011' in count):
    total += count['011']
if('101' in count):
    total += count['101']
if('110' in count):
    total += count['110']
probability_winning = total/shots
print("Probability of winning is:", probability_winning)
```

前述代码可以总结如下。在导入实现 GHZ 量子游戏所需的模块之后，然后构建游戏的电路。随后使用 'qasm_simulator' 模拟电路，并显示获得的结果。

到目前为止，我们在本小节中介绍了 GHZ 量子游戏。在下一小节中，我们将介绍 XOR 量子游戏。

## 理解 XOR 游戏

就像 CHSH 和 GHZ 游戏一样，XOR 游戏也是非局域量子游戏的一个例子。此外，XOR 游戏是非局域量子游戏中最受研究且最简单的游戏。

密码学首先通过将普通信息（称为明文）转换为不可理解的字符字符串（称为密文）来进行。这种转换过程是为了确保从发送方（通常称为 Alice）传输的信息不会在途中被窃听者（通常称为 Eve）截获，到达接收方（通常称为 Bob）。

一旦明文被转换为密文，它就可以通过信道传输给 Bob。在信道中，Eve 可以访问此类密文，但无法理解此类信息（密文），因为它是不可理解的。在接收方（Bob 侧），密文随后被转换回明文。

从之前讨论的细节可以看出，密码学可以分为两个过程。这些过程是加密和解密。前者（加密）包括使用加密密钥将明文转换为密文。另一方面，后者（解密）包括在加密密钥的帮助下将密文转换为明文。

密码系统（密码系统）的示意图如下所示：

![](img/2508606813a9204c61e8132ec0695cc1_175_0.png)

密码学的主要目标可以总结如下：

- **保密性（秘密性）**：确保信息无法被非法通信方访问。因此，秘密性确保信息没有未经授权的访问。
- **数据完整性**：确保信息在传输过程中不会被非法通信方修改。
- **认证**：保证信息的发送者就是他们声称的那个人。这反过来又确保唯一可以合法通信的各方是经过验证为真实的各方。
- **不可否认性**：防止通信中涉及的实体之一否认参与了此类通信。

根据所使用加密密钥的性质，密码学可以大致分为两个分支，即：

- 对称密码学（秘密密钥密码学）
- 非对称密码学（公钥密码学）

对称密码学使用一个由 Alice 和 Bob（请记住 Alice 和 Bob 是合法通信方）共享的加密密钥。为了保证 Alice 和 Bob 之间共享的信息得到保护，只要信息保持秘密，所使用的加密密钥就必须保持秘密。

现在我们已经提供了对称密码学的简要描述，是时候将注意力转向非对称密码学了。在密码学的这一分支中，使用两个单独但相关的加密密钥。其中一个密钥称为私钥，保持秘密，而另一个密钥称为公钥，被公开（因此可以通过不安全但经过身份验证的通信信道传输）。

在提供了有关密码学的一些背景信息后，现在是时候简要探讨密码学的历史了。因此，下一个小节将介绍密码学的历史。

## 古典密码学史

在历史上，密码学已被用于各种目的，例如以下用途：

- 外交通信
- 军事行动
- 商业活动
- 私人通信
- 宗教应用

早期实现的密码学，即古典密码学，主要关注消息中字符的替换。古典密码学中最著名的方案之一是凯撒替换密码。

## 凯撒密码

凯撒密码加密方案使用循环字母表，并用字母表中后面三个位置的另一个字符替换一个字符。因此，字符 *a* 将被替换为字符 *d*。

