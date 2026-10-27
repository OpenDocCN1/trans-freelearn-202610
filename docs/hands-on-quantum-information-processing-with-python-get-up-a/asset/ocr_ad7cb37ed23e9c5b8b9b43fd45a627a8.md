+   6. Finally, simulate the operation of the Bernstein-Vazirani algorithm using qasm_simulator:

```
simulator = Aer.get_backend('qasm_simulator')
result = execute(circuit, backend=simulator,
    shots=1024).result()
plot_histogram(result.get_counts(circuit))
```

The preceding code consists of four key stages. The first one initializes the qubits in both registers. This is then followed by applying the Hadamard gate to all the qubits in both registers. Furthermore, stage three involves the implementation of the oracle, which is made up of the CNOT gate. Finally, the last stage measures all the qubits in the input register.

The output of the Bernstein-Vazirani algorithm (using the input bit string 100) is shown as follows:

![](bbox=[0.27, 0.42, 0.72, 0.66])

Figure 5.7 – The output of the Bernstein-Vazirani algorithm for an input bit string 100

So far, we have covered three quantum algorithms in this chapter. In this section, we have learned about the Bernstein-Vazirani algorithm, and have seen how such an algorithm can be used to find a secret bit string. In the next section, we will cover two other techniques that play a crucial role in the quantum algorithms to be discussed later in this chapter. These techniques are quantum Fourier transform (QFT) and quantum phase estimation.