![](bbox=[0.38, 0.11, 0.62, 0.21])

Figure 4.9 – The quantum circuit for encoding the qubit |ψ> using two redundant qubits, with both redundant qubits being initialized to |0>

Essentially, the preceding circuit encodes the physical qubit:

|ψ> = α|0> + β|1>

with the logical qubit – α|000> + β|111>. The two other qubits, initialized to |0>, are used to provide redundancy, in order to enable the recovery of the original quantum states. These qubits are called auxiliary qubits. Alternatively, the auxiliary qubits are referred to as ancilla qubits, an unfortunate expression that will not be used in this book.

On the other hand, the circuit diagram for a qubit phase-flip quantum error-correction encoding circuit is shown as follows:

![](bbox=[0.28, 0.5, 0.68, 0.62])

Figure 4.10 – The quantum circuit for implementing the qubit phase-flip quantum error-correction encoding

Finally, the Python code for implementing quantum repetition code error correction using qiskit is given as follows:

> Note
> The link for this code can be found here: https://github.com/PacktPublishing/Hands-On-Quantum-Information-Processing-with-Python/tree/master/Chapter04.

+   1. The first step is to import all the packages that are required for the implementation of this quantum error correction scheme:

```
from qiskit import *
from qiskit.ignis.verification.topological_codes\
import RepetitionCode
```