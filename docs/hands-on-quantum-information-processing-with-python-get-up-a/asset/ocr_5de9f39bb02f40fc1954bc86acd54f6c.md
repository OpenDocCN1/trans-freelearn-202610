The Python code for implementing the matching pennies game using python, nashpy, and numpy is given as follows:

```
import nashpy as nash
import numpy as np

Alice = np.array([[1, -1], [-1, 1]])
Bob = np.array([[-1, 1], [1, -1]])
missing_pennies= nash.Game(Alice, Bob)
print(missing_pennies)

Alice_sigma = np.array([1, 0])
Bob_sigma = np.array([1, 0])
print(missing_pennies[Alice_sigma, Bob_sigma])

for eq in missing_pennies.support_enumeration():
    print(eq)
```

The preceding code can be summarized as follows. First, the Python modules necessary for the implementation of the matching pennies game are imported. This is then followed by the construction of the pay-off matrix, which, in turn, is followed by the calculation of both the payoffs and the Nash equilibrium.

In this subsection, we have covered one of the examples of zero-sum non-cooperative games, namely, the matching pennies game. In the next subsection, we will cover another example of the zero-sum non-cooperative game, namely, the rock-paper-scissors game.

# Exploring the rock-paper-scissors game

Another example of the zero-sum non-cooperative game is the famous rock-paper-scissors game. In this game, there are two players choosing the moves from three options, namely, the rock, the paper, and the scissors. In this zero-sum game, we can have the following outcomes:

+   - The rock beats the scissors.
+   - The scissors beat the paper.
+   - The paper beats the rock.