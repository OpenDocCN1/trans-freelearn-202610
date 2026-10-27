So far, we have covered three QKD protocols, namely, the BB84, the B92, and the SARG04. All these QKD protocols are examples of the prepare-and-measure category of QKD schemes. Next, we will discuss another protocol, namely, the E91 protocol. Unlike the previous three QKD protocols, the E91 protocol is an example of the entanglement-based category of the QKD schemes.

# The E91 QKD protocol

As already stated, the E91 protocol uses quantum entanglement as a quantum resource in order to secure the data. Additionally, this protocol was developed by Ekert, building on the ideas proposed by David Deutsch (Deutsch was mentioned earlier in this book, in Chapter 5, Quantum Algorithms).

The E91 QKD protocol operates as follows. First, both Alice and Bob share EPR pairs. Then they perform measurements on the parts of the pairs that are on their side. Since entanglement states are correlated, any measurement of the EPR pairs by either Alice or Bob would result in a correlated state on the side of the other communicating party (Bob or Alice).

The following steps show the implementation of the E91 protocol using Python:

+   1. First, import the modules necessary for the implementation of the E91 protocol:

```
from qiskit import *
import numpy as np
import matplotlib.pyplot as plt

np.random.seed(42)
```

+   2. Then, both Alice and Bob choose their bases to use:

```
A = [0, np.pi/8, np.pi/4]
B = [0, np.pi/8, -1*np.pi/8]
basesA = []
basesB = []
output = []
```

+   3. The next step involves defining the circuit to be used by Alice and Bob in order to generate a 100-bit random key:

```
for i in range(100):
    circ = QuantumCircuit(2, 2)
```