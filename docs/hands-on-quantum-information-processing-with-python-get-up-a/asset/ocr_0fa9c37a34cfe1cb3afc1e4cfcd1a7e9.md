As already stated, Shor’s algorithm efficiently finds the period of a periodic function. A periodic function, f(x), is given as follows:

```
f(x) = a^x mod N
```

Here, mod is a modulo operator, a and N are positive integers that have no common factors (thus a and N are co-prime), and the following applies:

```
a < N
```

Additionally, a period, r, is the smallest non-zero integer such that the following applies:

```
a^r mod N = 1
```

The implementation of Shor’s algorithm can be summarized as follows:

+   1. First, two registers are used.
+   The first register consists of m qubits, while the second register consists of n qubits, where the following applies:

```
m = 2n
```

The circuit for implementing Shor’s algorithm is shown in the following figure:

![](bbox=[0.22, 0.7, 0.75, 0.8])

Figure 5.9 – A quantum circuit for implementing an n-qubit Shor’s algorithm

Both registers of Shor’s algorithm are initialized to the |0> state for all the qubits. Therefore, the initial state is as follows:

```
ψ₀⟩ = |0⟩ₘ ⊗ |0⟩ₙ
```

+   2. This step is then followed by the application of the Hadamard gate to each of the m qubits in the first register. The state of the quantum system then changes to the following:

```
ψ₁⟩ = H|0⟩ ⊗ ⋯ ⊗ |0⟩ ⊗ |0⟩ₙ
```