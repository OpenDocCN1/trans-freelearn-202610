2. Now, we construct the quantum circuit for implementing the quantum phase estimation technique. This quantum circuit will then be used to execute the quantum phase estimation for two, four, and eight qubits:

```
def run_estimate(unknown_gate, qnum, repetitions):
    ancilla = cirq.LineQu-1)
    qubits = cirq.LineQubit.rangem)

    oracle_raised_to_power= [
        unknown_gate.on(ancilla).controlled_by\
        (qubits[i])**i
        for i in range(qnum)
    ]
    circuit = cirq.Circuit(cirq.H.on_each(*qubits),
                           oracle_raised_to_power,
                           cirq.QFT(*qubits,
                                   without_reverse=True)**-1,
                           cirq.measure(*qubits,
                                        key='phase'))
    return cirq.sample(circuit, repetitions=repetitions)

def experiment(qnum, repetitions):
    def example_gate(phi):
        gate = cirq.MatrixGate(
            matrix=np.array([
                [np.exp(2 * np.pi * 1.0j * phi), 0],
                [0, 1]]))
        return gate
```

3. The next step is to test the accuracy of the quantum phase estimation using the root mean square (RMS) error as the metric. The lower the RMS, the better the estimation:

```
print(f'Testing with {qnum} qubits.')
errors = []
for target in np.arange(0, 1, 0.1):
    result = run_estimate(example_gate(target),
                          qnum, repetitions)
```