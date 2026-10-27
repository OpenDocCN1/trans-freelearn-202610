
## Further reading

- Nielsen, M. A and Chuang, I. L. (2011) *Quantum computation and quantum information: 10th Edition.* New York, NY, USA: Cambridge University Press, 1107002176, 9781107002173.
- Bernhardt, C. (2019) *Quantum computing for everyone.* MIT Press.
- Abraham Asfaw, Luciano Bello, Yael Ben-Haim, Sergey Bravyi, Lauren Capelluto, Almudena Carrera Vazquez, Jack Ceroni, Richard Chen, Albert Frisch, Jay Gambetta, Shelly Garion, Leron Gil, Salvador De La Puente Gonzalez, Francis Harkins, Takashi Imamichi, David McKay, Antonio Mezzacapo, Zlatko Minev, Ramis Movassagh, Giacomo Nannicini, Paul Nation, Anna Phan, Marco Pistoia, Arthur Rattew, Joachim Schaefer, Javad Shabani, John Smolin, Kristan Temme, Madeleine Tod, Stephen Wood, and James Wootton. (2020). *Learn Quantum Computation Using Qiskit.* IBM. Available online: http://community.qiskit.org/textbook.

## Section 3: Deep Diving into Quantum Information

This section discusses the strategies that players can use in a non-local game and then proceeds to implement examples of CHSH and GHZ games. We will also showcase applications of quantum cryptography and Python implementations of certain quantum key distribution schemes. Furthermore, we will cover the key concepts and ideas in continuous-variable quantum information processing (QIP). Finally, we will talk about current research being undertaken, developments by various vendors, and what the future holds for the QIP domain.

This section comprises the following chapters:

- Chapter 6, Non-Local Quantum Games
- Chapter 7, Quantum Cryptography
- Chapter 8, Quantum Machine Learning
- Chapter 9, Continuous-Variable Quantum Information Processing
- Chapter 10, Current Trends in Quantum Information Processing

## 6 Non-Local Quantum Games

This chapter covers another aspect of quantum information processing, namely, **quantum game theory**. Furthermore, in this chapter, we will showcase tasks in which quantum mechanics can allow participants (players) to perform more optimally than if they made no use of quantum resources.

In this chapter, we will first provide an introduction to classical game theory and then proceed to quantum game theory. Furthermore, we will discuss the strategies that players can use in non-local quantum games and proceed to implement examples of *CHSH* and *GHZ* games.

In this chapter, we will cover the following main topics:

- Understanding classical game theory
- Understanding quantum game theory
- Understanding non-local quantum games
- Understanding the CHSH game
- Understanding the GHZ game
- Understanding the XOR game

### Learning about strategies in game theory

As already stated in this chapter, a strategy is a set of actions that each player takes in the game. Strategies are a significant component of game theory. A strategy gives a complete specification of a player's behavior. This (complete) specification describes actions that a player would take at each of their decision points (information sets).

In essence, for an n-player game, a strategy is a function that assigns each of the players' information sets (places where a player makes decisions) to an action available to said player at that information set. Therefore, it can be seen that a strategy can be thought of as a prescription for how to play a game in all possible scenarios.

Consider an n-player game. In such a setting, each player, i, has their own set of possible strategies, denoted as S_i. In order to play a game, each player, i, then selects the strategy:

s_i ∈ S_i

In such a setting, a vector of strategies (strategy set) selected by the players is given by:

s = (s_1, ⋯, s_n)

It is worth noting that a vector of strategies selected by the players in an n-player game ultimately determines the outcome for each player in the game.

A strategy that results in the player having the best outcome regardless of the strategies from other players is called the dominant strategy. On the other hand, a strategy that results in the worst outcome regardless of the strategies of other players is known as the dominated strategy.

Another comparison of strategies in game theory involves whether the strategy is deterministic. If the strategy of a player involves a deterministic set of actions, it is said to be a pure strategy. On the other hand, if the strategy is stochastic (in which case, the player chooses randomly among a set of possible actions), then the strategy is said to be a mixed strategy.

In this subsection, we have covered the strategies used in game theory. Furthermore, we have briefly discussed the dominant strategy. Finally, we have contrasted the pure and mixed strategies. The following section will explore the differences between cooperative games and the non-cooperative games.

Using the pay-off matrix, the two-player prisoner's dilemma game between Alice and Bob can be given as follows:

| Alice \ Bob | Confesses | Does not confess |
|---|---|---|
| Confesses | (3, 3) | (1, 4) |
| Does not confess | (4, 1) | (2, 2) |

Table 6.1 – A pay-off matrix for a two-player prisoner's dilemma game

Using the Python, NumPy, and Nashpy libraries, the code snippet for implementing a two-player prisoner's dilemma is given as follows:

```python
import nashpy as nash
import numpy as np

Alice = np.array([[3, 1], [4, 2]])
Bob = np.array([[3, 4], [1, 2]])
prisoner_dilemma = nash.Game(Alice, Bob)
print(prisoner_dilemma)

Alice_sigma = np.array([1, 0])
Bob_sigma = np.array([1, 0])
print(prisoner_dilemma[Alice_sigma, Bob_sigma])

for eq in prisoner_dilemma.support_enumeration():
    print(eq)
```

As you can see from the preceding code snippet, the first step is to import the two Python libraries, namely, NumPy and Nashpy, with the former being given an alias of np, and the latter being given an alias of nash.

After importing the necessary libraries, the next step is to construct the pay-off matrix for both Alice and Bob. This is followed by the creation of the prisoner's dilemma game using the nash.Game class. After creating the game, the utilities (payoffs) are then calculated, followed by a calculation of the Nash equilibrium.

The Python code for implementing the matching pennies game using Python, Nashpy, and NumPy is given as follows:

```python
import nashpy as nash
import numpy as np

Alice = np.array([[1, -1], [-1, 1]])
Bob = np.array([[-1, 1], [1, -1]])
matching_pennies = nash.Game(Alice, Bob)
print(matching_pennies)

Alice_sigma = np.array([1, 0])
Bob_sigma = np.array([1, 0])
print(matching_pennies[Alice_sigma, Bob_sigma])

for eq in matching_pennies.support_enumeration():
    print(eq)
```

The preceding code can be summarized as follows. First, the Python modules necessary for the implementation of the matching pennies game are imported. This is then followed by the construction of the pay-off matrix, which, in turn, is followed by the calculation of both the payoffs and the Nash equilibrium.

In this subsection, we have covered one of the examples of zero-sum non-cooperative games, namely, the matching pennies game. In the next subsection, we will cover another example of the zero-sum non-cooperative game, namely, the rock-paper-scissors game.

### Exploring the rock-paper-scissors game

Another example of the zero-sum non-cooperative game is the famous rock-paper-scissors game. In this game, there are two players choosing the moves from three options, namely, the rock, the paper, and the scissors. In this zero-sum game, we can have the following outcomes:

- The rock beats the scissors.
- The scissors beat the paper.
- The paper beats the rock.

4. After applying the Hadamard, X, Z, and controlled-NOT gates as shown in the previous code block, the next step is to perform measurement on the two qubits and store the results in the two-bit classical register:

```python
qc.measure(q[0], c[0])
qc.measure(q[1], c[1])
qc.draw('mpl')
plt.show()
```

5. Finally, use qasm_simulator to simulate this quantum circuit on the local simulator:

```python
simulator = BasicAer.get_backend('qasm_simulator')
job = execute(qc, simulator, shots=1024)
result = job.result()
count = result.get_counts(qc)
plot_histogram(count)
plt.show()
```

The preceding code can be summarized as follows.

First, the necessary modules are imported. This is followed by the construction of the circuit to implement Grover's algorithm. This circuit can be summarized by the following figure:

![](img/2508606813a9204c61e8132ec0695cc1_146_0.png)

Figure 5.11 – A circuit for implementing Grover's algorithm

After constructing the Grover circuit, the execution of this circuit is simulated using qasm_simulator.
