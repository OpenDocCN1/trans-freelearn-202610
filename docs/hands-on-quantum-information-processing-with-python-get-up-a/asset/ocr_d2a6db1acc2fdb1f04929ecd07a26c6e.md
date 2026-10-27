We have the following when $f(0)=1$:

$$f(0)\rangle - 1 \oplus f(0)\rangle = 1\rangle - 0\rangle$$

The following can then be observed:

$$f(0)\rangle - 1 \oplus f(0)\rangle = (-1)^{f(0)}(0\rangle - 1\rangle)$$

The same analysis can also be applied to the function $f(1)$. Refer to the quiz at the end of this chapter for this exercise.

Now, armed with this intuition, we can represent the state after applying the unitary transformation $F$ as follows:

$$\psi_3\rangle = \frac{1}{2}[0\rangle \otimes (-1)^{f(0)}(0\rangle - 1\rangle) + 1\rangle \otimes (-1)^{f(1)}(0\rangle - 1\rangle)]$$

This can then be simplified to the following:

$$\psi_3\rangle = \frac{1}{2}[(-1)^{f(0)}0\rangle + (-1)^{f(1)}1\rangle] \otimes [0\rangle - 1\rangle]$$

The next step is the application of the Hadamard gate to the first qubit. This results in the following:

$$\psi_4\rangle = \frac{1}{2}[((-1)^{f(0)} + (-1)^{f(1)})0\rangle + ((-1)^{f(0)} - (-1)^{f(1)})1\rangle] \otimes (0\rangle - 1\rangle)$$

Finally, the measurement is performed on the first qubit. It should be clear by now that if the function is constant, then the measured state would be 0 whereas if the function is balanced, the measured state would be 1.

Having provided an overview of Deutsch's algorithm, it is now time to explore how this algorithm can be implemented.

**Note**

This code is adopted from https://github.com/quantumlib/Cirq/blob/master/examples/deutsch.py.

The link for this code can be found here: https://github.com/PacktPublishing/Hands-On-Quantum-Information-Processing-with-Python/tree/master/Chapter05.