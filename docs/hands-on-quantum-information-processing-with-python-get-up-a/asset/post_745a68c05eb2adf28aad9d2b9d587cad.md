
In this section, we have provided two key inequalities in quantum mechanics, namely, Bell's inequality and CHSH inequality. We have also stated in this section that quantum mechanics violates both these inequalities. The following section will cover composite quantum systems and the entangled quantum systems.

## Understanding composite systems and entanglement

We have seen in Chapter 2, Quantum States, Operations, and Measurements, that composite quantum systems can either be separable or not separable. Let's consider two qubits:

|u₁⟩ = α₀|0⟩ + α₁|1⟩

and

|u₂⟩ = β₀|0⟩ + β₁|1⟩

As we have already seen in the previous section, if the two states are separable, then the composite system of these qubits will be given as follows:

|u⟩ = α₀β₀|00⟩ + α₀β₁|01⟩ + α₁β₀|10⟩ + α₁β₁|11⟩

Since the composite system |u⟩ is separable, it is possible to factorize (decompose) it into its constituent quantum states, namely, |u₁⟩ and |u₂⟩. However, this decomposition is not always possible. For instance, consider the following composite system:

|v⟩ = 1/√2 (|00⟩ + |11⟩)

Now, we want to see whether it is possible to decompose |v⟩ into its constituent quantum states, say |v₁⟩ and |v₂⟩. Recall that states |v₁⟩ and |v₂⟩ can be written as follows:

|v₁⟩ = α₀|0⟩ + α₁|1⟩

and

|v₂⟩ = β₀|0⟩ + β₁|1⟩

Now, if |v⟩ is a product state, it means the following:

$|v\rangle = \alpha_0\beta_0|00\rangle + \alpha_0\beta_1|01\rangle + \alpha_1\beta_0|10\rangle + \alpha_1\beta_1|11\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)$

This can only be the case if the following apply:

$\alpha_0\beta_1 = \alpha_1\beta_0 = 0$

and

$\alpha_0\beta_0 = \alpha_1\beta_1 = \frac{1}{\sqrt{2}}$

However, the two aforementioned equations cannot both be true. The implication of the first equation is that at least one of the probability amplitudes $\alpha_0$, $\alpha_1$, $\beta_0$, or $\beta_1$ is zero, while the second equation implies that neither of the probability amplitudes is zero. This leads to a contradiction. Therefore, the composite state |v⟩ cannot be decomposed to its constituent quantum states, |v₁⟩ and |v₂⟩. Therefore, |v⟩ is not separable and quantum states |v₁⟩ and |v₂⟩ are said to be entangled.

In this section, we have shown how quantum states can be entangled, instead of being separable states. The next section discusses how the **CNOT gate** is used as an entangling gate. The CNOT gate is used to entangle two quantum states.

## Understanding the CNOT gate – the entangling gate

In *Chapter 2, Quantum States, Operations, and Measurements*, we gave a brief overview of a two-qubit gate called the **controlled-NOT (CNOT) gate**. The CNOT gate can be used for entangling quantum states. It is also referred to as the entangling qubit. As an example, consider two qubits, $|w_1\rangle$ and $|w_2\rangle$, both in state |0⟩. Now, when we apply the Hadamard gate (the Hadamard gate was introduced in *Chapter 2, Quantum States, Operations, and Measurements*) to $|w_1\rangle$, we get the following:

$|w_1\rangle \rightarrow H|0\rangle = \frac{1}{\sqrt{2}}[|0\rangle + |1\rangle]$

Furthermore, by applying the CNOT gate to |w₁⟩ in the equation and |w₂⟩ (which is still in state |0⟩), we get the following:

$|w_2\rangle \rightarrow \text{CNOT}(|w_1\rangle, |0\rangle) = \frac{1}{\sqrt{2}} (|00\rangle + |11\rangle)$

This, as we have already observed, is an entangled state. This state is one example of entangled quantum states known as Bell states, Bell basis states, or EPR states.

The Python code for generating this Bell state using qutip is shown as follows:

```
from qutip import *

w1 = bell_state(state="00")
print(" A matrix for Bell pair generation is:\n", w1)
```

The code uses the bell_state() function from qutip to create one of the four Bell states.

The following code snippet shows the output of this code:

```
A matrix for the Bell pair generated is:

Quantum object: dims = [ [2, 2], [1, 1] ],
                  shape = (4, 1), type = ket
Qobj data =
[ [0.70710678 ]
  [0.          ]
  [0.          ]
  [0.70710678] ]
```

Let's now move on to the next section and learn about Bell states.

## Understanding Bell states

As we have already mentioned, the |w₂> state in the *Understanding the Bell theorem and CHSH inequality* section is an example of a Bell state. There are other examples of Bell states too (there are actually four Bell states). The Bell states are prepared by applying the Hadamard gate to the first input qubit, and then applying the CNOT gate on the result to the first qubit and the second qubit input. This can be summarized as follows:

```
|ψ₀₀⟩ = CNOT(H|0⟩, |0⟩) = 1/√2 (|00⟩ + |11⟩)
|ψ₀₁⟩ = CNOT(H|0⟩, |1⟩) = 1/√2 (|00⟩ − |11⟩)
|ψ₁₀⟩ = CNOT(H|1⟩, |0⟩) = 1/√2 (|01⟩ + |10⟩)
|ψ₁₁⟩ = CNOT(H|1⟩, |1⟩) = 1/√2 (|01⟩ − |10⟩)
```

It is left as an exercise for you to prove that the equations provided are correct.

Let's discuss the four Bell states:

- As can be seen, the first Bell state, |ψ₀₀⟩, is created when the two input qubits are in state |0⟩.
- Furthermore, the Bell state |ψ₀₁⟩ is created when the first input qubit is in state |0⟩ and the second input qubit is in state |1⟩.
- Additionally, the Bell state |ψ₁₀⟩ is created when the first input qubit is in state |1⟩ and the second input qubit is in state |1⟩.
- Finally, the Bell state |ψ₁₁⟩ is created when both input qubits are in state |1⟩.

The Python code for generating these Bell states using qutip is shown as follows:

```
from qutip import *

v_00 = bell_state(state="00")
v_01 = bell_state(state="01")
v_10 = bell_state(state="10")
v_11 = bell_state(state="11")

print("v_00 is:", v_00)
print("v_01 is:", v_01)
```

```
print("v_10 is:", v_10)
print("v_11 is:", v_11)
```

The preceding code snippet demonstrates the generation of the four Bell states using the bell_state() function from qutip.

Finally, the simple quantum circuit for creating a Bell state is shown here (quantum circuits will be discussed in detail in Chapter 4, Working with Quantum Circuits):

![](img/2508606813a9204c61e8132ec0695cc1_79_0.png)

Figure 3.1 – Quantum circuit for creating a Bell state

In the preceding circuit, the Hadamard gate acts on the first qubit (|a⟩), and the CNOT gate acts on the output of H|a⟩ and the second input qubit (|b⟩). As an exercise, show that the preceding circuit actually generates the Bell states when the first qubit |a⟩ = {|0⟩, |1⟩} and the second qubit |b⟩ = {|0⟩, |1⟩}.

In this section, we have demonstrated how Bell states are generated using the CNOT gate. In the next two sections, we will cover the entanglement of more than two quantum states. The next section will focus on the generation of the GHZ state. This will then be followed by a discussion of the generation of the W state.

## Understanding the entanglement of more than two quantum states

In the previous section, we demonstrated how Bell states are generated using the CNOT gate. In this section, we will cover the entanglement of more than two quantum states. The next subsection will focus on the generation of the GHZ state. This will then be followed by a discussion of the generation of the W state.

## The W state

Besides the GHZ state, another composite quantum state that can be formed by the entanglement of three or more qubits is called the **W state**. A three-qubit W state is mathematically represented as follows:

$$\psi_W = \frac{1}{\sqrt{3}}(|100\rangle + |010\rangle + |001\rangle)$$

In general, the W state generated from entangling $n$ qubits is given as follows:

$$\psi_W = \frac{1}{\sqrt{n}}(|10\cdots0\rangle + |01\cdots0\rangle + \cdots + |0\cdots01\rangle)$$

Finally, the Python code snippet for generating the three-qubit W state using qutip and numpy is shown here:

```
from qutip import *
import numpy as np

a = np.array([[1], [0]])
b = np.array([[0], [1]])

a = Qobj(a)
b = Qobj(b)

W = (1/np.sqrt(3)) * (tensor(b, a, a) + \
                    tensor(a, b, a) + \
                    tensor(a, a, b))

print("The W state is:", W)
```

As can be observed, the preceding code uses two modules, namely, qutip and numpy. numpy is used to create two vectors, namely, a and b, using the array() function. These vectors are then converted into the qutip objects using the Qobj() function. Then, the W state is generated, and the W variable is assigned to such a state.

Now that we have discussed many-particle entanglement using either the GHZ states or the W states, the next section will discuss how quantum entanglement can be used as a resource in quantum information processing.
