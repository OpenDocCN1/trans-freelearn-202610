In the example, the graph on the left-hand side shows the points that satisfy the equation shown here:

$$y^2 = x^3 + 7x + 11$$

There are 27 solutions to the equation shown earlier over a finite field $F_{23}$. $P$ and $Q$ are chosen to be added to produce the point $R$. Calculations are shown on the right-hand side, which calculates the third point $R$. Note that here, $l$ is used to depict the line going through $P$ and $Q$.

As an example to show how the equation is satisfied by the points shown in the graph, a point $(x, y)$ is picked up where $x = 3$ and $y = 6$.

Using these values in the equation shows that the equation is satisfied indeed. This is shown as follows:

$$y^2 \mod 23 = x^3 + 7x + 11 \mod 23$$

$$6^2 \mod 23 = 3^3 + 7(3) + 11 \mod 23$$

$$36 \mod 23 = 59 \mod 23$$

$$13 = 13$$

The next section will introduce the concept of point doubling, which is another operation that can be performed on elliptic curves.

# Point doubling

The other group operation on elliptic curves is called point doubling and is described in the following diagram. This is a process where $P$ is added into itself. In this method, a tangent line is drawn through the curve, as shown in the following graph. The second point is obtained, which is at the intersection of the tangent line drawn and the curve. This point is then mirrored to yield the result, which is shown as $2P = P + P$: