The preceding equation in our case can be summarized as follows:

$$\left\{ S_T^1, S_T^2, S_T^3 \right\}$$

This entire process is illustrated in the following diagram:

![](bbox=[0.27, 0.228, 0.729, 0.458])

+   2. The second phase is the payoff phase.

In this phase, we use the values of the underlying at maturity and for each of them, we compute the value of the payoff, as follows:

$$\left\{ H \left( S_T^k \right) \right\} \quad k = 1...N+1$$

In our case, the equation can be summarized as follows:

$$\left\{ H(S_T^1), H(S_T^2), H(S_T^3) \right\}$$

The following in turn are the values of the option at maturity T:

$$\left\{ V_T^k = H \left( S_T^k \right) \right\} \quad k = 1...N+1$$

In our case, the preceding equation can be summarized as follows:

$$\left\{ V_T^1 = H(S_T^1), V_T^2 = H(S_T^2), V_T^3 = H(S_T^3) \right\}$$