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

4. Finally, simulate the CHSH circuit using 'qasm_simulator' and display the probability of winning:

```
```python
backend = Aer.get_backend('qasm_simulator')
print(winning_probability(backend, shots=1024))
```
```

The preceding code can be summarized as follows. First, the necessary Python modules are imported. This is then followed by construction of the CHSH quantum circuit, using two qubits (for Alice and Bob). This quantum circuit starts with both qubits being initialized to the quantum state, |0>. This is then followed by the creation of the Bell pair. Furthermore, depending on the classical bits *x* and *y* for Alice and Bob, respectively, appropriate rotation gates are applied by both Alice and Bob. Eventually, both Alice and Bob measure their respective qubits.

Following construction of the quantum circuit for implementing the CHSH game, the next step in the preceding Python code is to simulate the circuit using 'qasm_simulator'. The results obtained are then displayed.

In this subsection, we have covered the CHSH quantum game. We have also provided a Python code for implementing the CHSH game. In the next subsection, we will explore the GHZ game.

# Understanding the GHZ game

Just like the CHSH quantum game discussed previously, the **Greenberger-Horne-Zeilinger (GHZ)** quantum game is also an example of a non-local quantum game. It is used to witness the tripartite entanglement in quantum systems. However, unlike the CHSH game, the GHZ game is a three-player game (Alice, Bob, and Charlie are the players).