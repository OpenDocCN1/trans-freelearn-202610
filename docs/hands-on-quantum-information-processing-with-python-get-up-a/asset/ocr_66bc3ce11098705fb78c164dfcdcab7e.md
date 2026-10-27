As it can be seen from the previous figure, the qubits are first initialized to state |0>. Therefore, the first state of the preceding system is as follows:

ψ₀⟩ = |0⟩ ⊗ |0⟩

This state is then evolved by flipping the second qubit. This is achieved by applying the X gate to the second qubit. The new state of the system then becomes the following:

ψ₁⟩ = |0⟩ ⊗ |1⟩

This step is followed by the application of the Hadamard gate on each of the qubits, resulting in the following:

ψ₂⟩ = H|0⟩ ⊗ H|1⟩

This can also be given as follows:

ψ₂⟩ = 1/2 (|00⟩ − |01⟩ + |10⟩ − |11⟩)

Now, the next step is to apply the unitary transformation F on the preceding state. This results in the following state:

ψ₃⟩ = 1/2 (|0⟩ ⊗ |f(0)⟩ − |0⟩ ⊗ |1 ⊕ f(0)⟩ + |1⟩ ⊗ |f(1)⟩ − |1⟩ ⊗ |1 ⊕ f(1)⟩)

Now we have to consider two scenarios. The first scenario is when the function being evaluated is a constant function. This, as we have already seen, means that f(0) = f(1). Therefore, the preceding state would be simplified to the following:

ψ₃⟩ = 1/2 [(|0⟩ + |1⟩) ⊗ (|f(0)⟩ − |1 ⊕ f(0)⟩)]

The next step of Deutsch's algorithm involves applying the Hadamard gate to the first qubit of the preceding state. This results in the following:

ψ₄⟩ = 1/√2 [|0⟩ ⊗ (|f(0)⟩ − |1 ⊕ f(0)⟩)]

Finally, measurement is performed on the first qubit, resulting in bit 0. This is consistent with the fact that the function is constant, since we now know that for a constant function, the following applies:

f(0) ⊕ f(1) = 0