
The preceding outcomes are applicable for the players Alice and Bob, such that, if Alice chooses the scissors and Bob chooses the rock, Bob wins the game. On the other hand, if Alice chooses the paper and Bob chooses the rock, then Alice wins.

The pay-off matrix for the rock-paper-scissors game is shown as follows:

| Alice vs Bob | Rock | Paper | Scissors |
|---|---|---|---|
| Rock | (0,0) | (-1,1) | (1,-1) |
| Paper | (1,-1) | (0,0) | (-1,1) |
| Scissors | (-1,1) | (1,-1) | (0,0) |

Table 6.4 – A pay-off matrix for a rock-paper-scissors game

The Python code for implementing a rock-paper-scissors game using python, numpy, and nashpy is given as follows:

```
import nashpy as nash
import numpy as np
Alice = np.array([[0, -1, 1], [1, 0, -1], [-1, 1, 0]])
Bob = np.array([[0, 1, -1], [-1, 0, 1], [1, -1, 0]])
rock_paper_scissors = nash.Game(Alice, Bob)
print(rock_paper_scissors)

Alice_sigma = np.array([0, 0, 1])
Bob_sigma = np.array([0, 1, 0])

for eq in rock_paper_scissors.support_enumeration():
    print(eq)
```

The preceding code is similar in structure to those discussed earlier. It involves the importing of the necessary Python modules, which, in turn, is followed by the construction of the pay-off matrix. This is then followed by calculations of the utilities and the Nash equilibrium.

In this section, we learned about the history of game theory and the various game strategies. We also learned about cooperative and non-cooperative games, together with zero-sum and non-zero-sum games. This concludes the section on classical game theory. The next section will cover quantum game theory.

## Understanding quantum game theory

Basically, a quantum game can be conceived of as any quantum system that can be manipulated by two or more parties, and where the payoff of the moves from these parties can be reasonably quantified.

The quantum game consists of the following elements:

- The number of players, N
- The set of strategies, S
- The utility/payoff functionals, P
- The initial quantum state, ρ
- The Hilbert space, H (refer to Chapter 2, Quantum States, Operations, and Measurements, for further details on the Hilbert space)

In essence, quantum game theory generalizes and quantizes classical game theory, discussed in the previous section, to the quantum domain. Simply put, quantum game theory is the game-theoretic framework that makes use of ideas from quantum mechanics.

In quantum game theory, quantum mechanical concepts, such as the quantum superposition of states and entanglement, can be used in the following ways:

- Quantum superposition of initial states
- Quantum entanglement of initial states
- Quantum superposition of strategies

As already stated previously, quantum game theory quantizes classical games. There are several reasons for quantizing classical games. These are as follows:

- To tap into the already established classical games in order to develop a new framework of game theory. This is due to the fact that both classical games and quantum mechanics are based on probability theory.
- To investigate whether the game-theoretic effects can be observed at a sub-atomic level, where quantum mechanics dictates the rules.
- To investigate the connection between game theory and the theory of quantum communication.
- To explore whether quantum mechanics can offer an advantage in order to win some of the specially designed non-cooperative games.

The first quantum game was proposed by Meyer in 1999. In this work, Meyer proposed a quantum game called the penny flip coin. This game is closely related to the matching penny game discussed earlier. Furthermore, Meyer demonstrated in this work that through the use of a quantum strategy, a player can increase their expected payoff.

The work of Meyer was then shortly followed by the work of Eisert et. al., who further introduced quantum games and quantum strategies. Since these early contributions, there has been a significant progress in the development of a quantum game-theoretic framework.

So far, we have provided a brief introduction to quantum game theory. In the next subsection, we will focus more on quantum game theory by exploring non-local quantum games.

### Understanding non-local quantum games

A non-local quantum game is a game where the quantum players are in different locations when they are playing the game. Non-local games are typically used to test non-locality in quantum systems. A two-player non-local game involves two players (Alice and Bob) and the referee.

The role of the referee, in this case, is to ensure that neither player (Alice or Bob) communicates with the other while playing the game. However, Alice and Bob are allowed to entangle their qubits (to share correlations). Typically, in non-local quantum games, the players are allowed to determine a joint strategy from the complete knowledge of an input distribution.

In this subsection, we have covered quantum non-local games. In the next subsection, we will briefly explore quantum strategies in non-local quantum games.

### Exploring quantum strategies in non-local quantum games

In classical game theory, strategies can either be pure (deterministic) or mixed (probabilistic). On the other hand, in quantum game theory, the strategies can only be probabilistic.

The quantum strategy for each player, denoted by s_i, is a player's procedure for deciding which action to take, depending on the player's information. The quantum strategy space, S = {s_j}, is a set of strategies available to the player.

In order to probe the quantum strategies further, consider a two-player quantum game, τ, characterized by:

τ = (H, ρ, S_A, S_B, P_A, P_B)

where H is the Hilbert space, ρ is the initial quantum state, S_A and S_B are the strategies of the two players, and P_A and P_B are the pay-off functionals of these players. In such a case, quantum strategies s_A and s_B, which can be given as s_A ∈ S_A and s_B ∈ S_B, are the quantum operations.

A two-player quantum game is referred to as a zero-sum quantum game if the expected payoffs total zero for all pairs of quantum strategies. Otherwise (if this is not the case), it is called a non-zero-sum quantum game.

Furthermore, consider the strategies s_A, s'_A, s_B, and s'_B such that, s_A, s'_A ∈ S_A and s_B, s'_B ∈ S_B.

Furthermore, the quantum strategies s_A and s'_A are equivalent if P_A(s_A, s_B) = P_A(s'_A, s_B) and P_B(s_A, s_B) = P_B(s'_A, s_B) for all s_B.

Similarly, quantum strategies s_B and s'_B are equivalent if P_B(s_A, s_B) = P_B(s_A, s'_B) and P_A(s_A, s_B) = P_A(s_A, s'_B) for all s_A.

Additionally, a quantum strategy s_A is called a dominant strategy if:

P_A(s_A, s'_B) ≥ P_A(s'_A, s'_B)

In a similar manner, a quantum strategy s_B is called a dominant strategy if:

P_B(s'_A, s_B) ≥ P_B(s'_A, s'_B)

At this juncture, it is worth noting that a pair of quantum strategies (s_A, s_B) is said to be an equilibrium in strategies if s_A and s_B are the dominant strategies for the two players in this two-player quantum game.

Finally, a pair of quantum strategies (s_A, s_B) is called a Nash equilibrium if P_A(s_A, s'_B) ≥ P_A(s'_A, s'_B) and P_B(s'_A, s_B) ≥ P_B(s'_A, s'_B).

In this subsection, we have provided a brief exposition to non-local quantum strategies. In the next subsection, we will discuss one of the examples of non-local quantum games, namely, the Clauser-Horne-Shimony-Holt (CHSH) quantum game.

### Understanding the CHSH game

The CHSH quantum game is an example of a non-local quantum game. It uses the CHSH inequality that was discussed earlier in this book, in Chapter 3, Entanglement and Teleportation. It just turns inequality into a quantum game that is played by Alice and Bob, in the presence of the referee.

The CHSH quantum game can have an application in the witnessing of bipartite entanglement in quantum systems.

The CHSH quantum game can be summarized as follows. Consider two players, Alice and Bob. They respectively receive bits x and y from the referee, and they have to respond with bits a and b, respectively. Additionally, Alice and Bob win if:

$a \oplus b = x \cdot y$

with both Alice and Bob communicating on the joint strategy before the start of the game, and not being permitted to communicate during the game.

The following steps summarize the procedure used for implementing the CHSH game:

- Before the start of the game, Alice and Bob share a Bell state (Bell pair).
- If Alice receives x = 0, she measures her qubit and outputs the results.
- If she receives x = 1, she applies the rotation gate of π/2 along the y axis, Ry(π/2), and measures her qubit.
- If Bob receives y = 0, he applies the rotation gate Ry(π/4) and then measures his qubit.
- If Bob receives y = 1, he applies the rotation gate Ry(-π/4) and then measures his qubit.

The following steps demonstrate implementation of the CHSH game using Python:

1. Import the necessary modules:

```
from qiskit import *
import numpy as np

np.random.seed(42)
```
