```
CNOT|10⟩ → |11⟩
```

and

```
CNOT|10⟩ → |11⟩.
```

Mathematically, a CNOT gate is represented as:

```
CNOT = |0⟩⟨0| ⊗ I + |1⟩⟨1| ⊗ X.
```

Finally, since a CNOT gate is an operator, its matrix is given as:

|1 0 0 0|
|0 1 0 0|
|0 0 0 1|
|0 0 1 0|

The CNOT gate can be thought of as applying the X gate conditioned on the state of the control qubit. That being said, there are other two-qubit gates that apply the single-qubit operation on the target qubit conditioned on the state of a control qubit.

Examples of these two-qubit conditional gates include a controlled Z gate and controlled S gate. The operations and matrix representations of these two gates are left as an exercise to the reader.

# Swap gate

Another two-qubit quantum gate is the Swap gate. As the name implies, the Swap gate swaps the positions of the qubits such that for a computational basis state, |a,b⟩, we have the following:

```
Swap|a,b⟩ → |b,a⟩.
```

The matrix representation of a Swap gate is as follows:

Swap = |1 0 0 0|
|0 0 1 0|
|0 1 0 0|
|0 0 0 1|.