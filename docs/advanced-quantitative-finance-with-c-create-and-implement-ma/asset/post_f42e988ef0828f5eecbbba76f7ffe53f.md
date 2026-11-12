
We now use the specific form of the payoff to describe a European Call option as follows:

{max(S₄¹ - K, 0), max(S₄² - K, 0), max(S₄³ - K, 0), max(S₄⁴ - K, 0), max(S₄⁵ - K, 0)}

We apply the following numbers to the preceding equation to get the following result:

{3.37, 0.00, 5.27, 8.24, 17.73}

The expected value of the preceding calculation is as follows:

E[H(S_T')] = 1/5(3.37 + 0.00 + 5.27 + 8.24 + 17.73) = 6.92

- 3. Now discount the expectation to the present.

We now use the following continuous compounding to discount the expected payoff we just calculated in step 2 in order to determine the value of the premium:

π = exp(-rT) × E[H(S_T)] = exp(-(0.05)(0.5)) × (6.92) = 6.75

In this example, we have used only five scenarios for our MC price. In practice, hundreds or even thousands of scenarios are required in order to obtain an acceptable error. Clearly, the more scenarios you use, the more accurate the approximation. It is possible to derive some error-bound formulas for the MC method and show the speed of convergence. For more details, the reader is invited to refer to Monte Carlo Methods in Financial Engineering. Putting together all the trajectories for the five MC scenarios, we obtain the table shown in the following screenshot. Here, we see that all the trajectories start at S0=100 and lead to some final value S5. For each trajectory, we compute the payoff H, which is then averaged to compute its expected value. The result is then discounted to obtain the present value of the derivative.

| S0 | S1 | S2 | S3 | S4 | S5 | H(S_T) |
|---|---|---|---|---|---|---|
| 100,00 | 101,06 | 107,10 | 115,81 | 113,49 | 103,37 | 3,37 |
| | 0,0884 | 0,8656 | 1,2070 | -0,3961 | -1,4890 | |
| 100,00 | 105,81 | 108,18 | 101,43 | 103,05 | 94,40 | 0,00 |
| | 0,8390 | 0,2757 | -1,0661 | 0,1735 | -1,4057 | |
| 100,00 | 100,17 | 102,71 | 99,83 | 105,65 | 105,27 | 5,27 |
| | -0,0515 | 0,3215 | -0,5230 | 0,8436 | -0,1360 | |
| 100,00 | 108,82 | 113,29 | 112,32 | 117,29 | 108,24 | 8,24 |
| | 1,3157 | 0,5703 | -0,2142 | 0,6198 | -1,2992 | |
| 100,00 | 103,79 | 102,30 | 101,87 | 112,90 | 117,73 | 17,73 |
| | 0,5200 | -0,3055 | -0,1464 | 1,6339 | 0,5973 | |
| 6,75 | | | | | mean | 6,92 |

Example of the Monte Carlo simulation

## The Binomial Trees method

**Binomial Trees (BT)** can be traced to the work of (Cox, Ross, and Rubinstein 1979). Like MC methods, they are based on the idea of how the discretization of stock prices can jump up or down. Unlike the MC methods, BT are not based on simulation of many possible paths, but on the construction of a single path of possible future prices that bifurcates at every node. These prices, as well as their associated probabilities, constitute the tree. Once this tree is built, the prices of the underlying at maturity can be determined, and the the payoff at maturity can be then computed and discounted to the present time in order to determine the premium of the derivative.

## Algorithm of the BT method

The BT method when applied to price derivatives is composed of three phases: the construction of the tree of prices (forward phase), the computation of the payoffs (maturity phase), and the discounting of the payoffs to the present time (backward phase). We will now explain the BT method in the simplified context of a two-step BT. This can be easily generalized to an _N_ step tree.

To start with, we assume that the underlying can only go up or down in the next time step. So we specify the up factor $u$ to describe how much the value today changes to a higher value and the down factor $d$ to describe how much the value today changes to a lower value, such that the up value is $S(T)=uS(0)$, and the down value is $S(T)=dS(0)$. Furthermore, refer to "Option pricing: A simplified approach". The formula for the up and down values can be shown as follows:

$$u = \exp(\sigma\sqrt{\Delta t})$$
$$d = \exp(-\sigma\sqrt{\Delta t})$$

The following is the formula for the probabilities of going up:

$$p = \frac{\exp(r\Delta t) - d}{u - d}$$

The probability of going down is $1-p$. We can now proceed to construct our binomial tree in the following three phases:

- 1. The first phase is the forward phase. Here we construct the tree. Like in MC simulation, time is discretized in steps $dt$ from $t=0$ to $t=T$. From one step $tp$, the next price of the underlying can either go up or down by a factor $u$ or $d$ as shown in the following formulas:

$$\text{At } t=0: S_0$$
$$\text{At } t=t_1 : uS_0 \text{ or } dS_0$$
$$\text{At } t=t_2 : u^2S_0, udS_0, d^2S_0$$

Thus, the values of the tree at maturity are as follows:

$$S_T^1 = u^2S_0, S_T^2 = udS_0, S_T^3 = d^2S_0$$

In general case, we proceed in a similar manner until we arrive at the maturity $T$, and we have $N+1$ values for the variable $S$. We will calculate these values with the help of the following equation:

$$\left\{ S_T^k \right\} \quad k=1...N+1$$

The preceding equation in our case can be summarized as follows:

$$\left\{ S_T^1, S_T^2, S_T^3 \right\}$$

This entire process is illustrated in the following diagram:

![](img/29d9d2cdc85db7e20e91367a93390be2_51_0.png)

- 2. The second phase is the payoff phase.

In this phase, we use the values of the underlying at maturity and for each of them, we compute the value of the payoff, as follows:

$$\left\{ H \left( S_T^k \right) \right\} \quad k = 1...N+1$$

In our case, the equation can be summarized as follows:

$$\left\{ H(S_T^1), H(S_T^2), H(S_T^3) \right\}$$

The following in turn are the values of the option at maturity T:

$$\left\{ V_T^k = H \left( S_T^k \right) \right\} \quad k = 1...N+1$$

In our case, the preceding equation can be summarized as follows:

$$\left\{ V_T^1 = H(S_T^1), V_T^2 = H(S_T^2), V_T^3 = H(S_T^3) \right\}$$

- 3. The third phase is the backward phase.

In this final phase, we take the values of the payoff at maturity and proceed in a backward manner. We move from the last node to the previous nodes, by computing the option value as the discounted expected payoff in the previous nodes using the weighted probabilities, as follows:

```
V^k_{T-1} = \exp(-r\Delta t)\big[p V^k_T + (1-p) V^k_T\big]
```

In our case, in the second step, the equations are as follows:

```
V^1_2 = \exp(-r\Delta t)\big[p V^1_3 + (1-p) V^2_3\big]
```

```
V^2_2 = \exp(-r\Delta t)\big[p V^2_3 + (1-p) V^3_3\big]
```

And, in the first step, the equation is as follows:

```
V^1_1 = \exp(-r\Delta t)\big[p V^1_2 + (1-p) V^2_2\big]
```

The premium of the derivative, the option price, is the value \(\pi = V^1_1\).

## Example of the BT method

Consider the example where we would like to price a six-month European Call option on Rolls Royce equity (R.R.L.). The current equity price of the stock is £100.00, with a volatility of 30 percent p.a. and a strike of £90. We assume the stock pays no dividends. The current risk-free rate is 5 percent pa. How do we proceed to solve this problem using BT?

To start with, we divide the life of the option in two steps, thus dt=0.25. The tables in the following screenshot illustrate the numerical values for each of the three steps applied to this problem:

| | | | |
|---|---|---|---|
| | | 134.99 | |
| | 116.18 | | |
| S0 100.00 | | 100.00 | |
| | 86.07 | | |
| | | 74.08 | |
| | | | ST |
| TIME | 0.00 | 0.25 | 0.50 |

| | | | |
|---|---|---|---|
| | | 44.99 | |
| | | 10.00 | |
| | | 0.00 | |
| | | | H(ST) |
| TIME | 0.00 | 0.25 | 0.50 |

| | | | |
|---|---|---|---|
| | | 44.99 | |
| | 27.30 | 10.00 | |
| V0 16.04 | | 4.98 | |
| | | 0.00 | |
| TIME | 0.00 | 0.25 | 0.50 |

Example of Binomial Trees pricing.

We first compute the up and down factors as well as the up probability \( p \). In numerical terms, these are calculated using the following equations:

\( u = \exp(\sigma\sqrt{\Delta t}) = \exp(0.30\sqrt{0.25}) = 1.16 \)
\( d = \exp(-\sigma\sqrt{\Delta t}) = \exp(-0.30\sqrt{0.25}) = 0.86 \)

The following are the probabilities of going up and down respectively:

\( p = \frac{\exp(r\Delta t) - d}{u - d} = \frac{\exp(0.05 \times 0.25) - 0.86}{1.16 - 0.86} = 0.50 \)

\( (1 - p) = 0.50 \)

With all these parameters, we can now proceed to construct our tree in three phases, as follows:

- 1. The first phase is the forward phase.

We can now construct the two levels of the tree as follows.

\( At\ t = 0 : S_0 = 100 \)
\( At\ t = t_1 : uS_0 = 116.18\ or\ dS_0 = 86.07 \)
\( At\ t = t_2 : u^2S_0 = 134.99,\ udS_0 = 100,\ d^2S_0 = 74.08 \)

Thus, the values of the tree at maturity are as follows:

\( \{ S_T^1 = 134.99, S_T^2 = 100, S_T^3 = 74.08 \} \)

- 2. The second phase is the payoff phase.

In this phase, we use the values of the underlying at maturity, and for each of them, we compute the value of the payoff, as follows:

\( \{ H \left( S_T^k \right) \} \quad k = 1...N+1 \)

In our case, the equation can be summarized as follows:

\( \{ H(S_T^1) = 44.99, H(S_T^2) = 10, H(S_T^3) = 0 \} \)
