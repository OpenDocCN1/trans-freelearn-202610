Given the complexity of the processes in the LMM, it is not possible to obtain a closed-form solution for all the forward rates. This is not a problem, however, as robust numerical methods can be used to solve the discretized version of the forward rate. One important method that is widely used for market models is Monte Carlo simulation. The forward rates Li(Tn) are the realizations of the spot LIBOR rates. In each column, the forward rate Li(Tn+1) is updated using the following discretization:

$$L_i(T_{n+1}) = L_i(T_n) - \sum_{k=i+1}^N \frac{\alpha_k \sigma_k(T_n) L_k(T_n)}{1 + \alpha_k L_k(T_n)} \sigma_i(t) L_i(T_n) (T_{n+1} - T_n) + \sigma_i(T_n) (W^{N+1}(T_{n+1}) - W^{N+1}(T_n))$$\nEquation 9

The preceding equation, like the ones we have shown for equities and forex, ought to be solved iteratively using simulation. The forward rates can be arranged in an arithmetic table as follows:

| L1(T0) | | | |
|---|---|---|---|
| L2(T0) | L2(T1) | | |
| L3(T0) | L3(T1) | L3(T2) | |
| L4(T0) | L4(T1) | L4(T2) | L4(T3) |

In the preceding table, the left column represents the term structure of interest rates at time t=0. Given a set of LIBOR rates realized on this for T1, T2, ...TN, we can extract the future rates that we need for simulation as the ones present in the main diagonal of the preceding table.

For example, consider the following paying fixed-for-floating **Interest Rate Swap** (IRS) with a notional of 1 million EUR. This IRS pays 5 percent every 3 months and receives EURIBOR 3 months' rate every 3 months. The total maturity of the swap is one year, given the following current term structure of interest rates:

| L1(0) | | | |
|---|---|---|---|
| L2(0) | L2(3M) | | |
| L3(0) | L3(3M) | L3(6M) | |
| L4(0) | L4(3M) | L4(6M) | L4(9M) |