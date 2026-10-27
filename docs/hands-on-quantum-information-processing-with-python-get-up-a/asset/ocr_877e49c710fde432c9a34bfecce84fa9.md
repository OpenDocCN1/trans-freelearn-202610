```
print("v_10 is:", v_10)
print("v_11 is:", v_11)
```

The preceding code snippet demonstrates the generation of the four Bell states using the bell_state() function from qutip.

Finally, the simple quantum circuit for creating a Bell state is shown here (quantum circuits will be discussed in detail in Chapter 4, Working with Quantum Circuits):

![](bbox=[0.26, 0.27, 0.68, 0.42])

Figure 3.1 – Quantum circuit for creating a Bell state

In the preceding circuit, the Hadamard gate acts on the first qubit (|a⟩), and the CNOT gate acts on the output of H|a⟩ and the second input qubit (|b⟩). As an exercise, show that the preceding circuit actually generates the Bell states when the first qubit |a⟩ = {|0⟩, |1⟩} and the second qubit |b⟩ = {|0⟩, |1⟩}.

In this section, we have demonstrated how Bell states are generated using the CNOT gate. In the next two sections, we will cover the entanglement of more than two quantum states. The next section will focus on the generation of the GHZ state. This will then be followed by a discussion of the generation of the W state.

# Understanding the entanglement of more than two quantum states

In the previous section, we demonstrated how Bell states are generated using the CNOT gate. In this section, we will cover the entanglement of more than two quantum states. The next subsection will focus on the generation of the GHZ state. This will then be followed by a discussion of the generation of the W state.