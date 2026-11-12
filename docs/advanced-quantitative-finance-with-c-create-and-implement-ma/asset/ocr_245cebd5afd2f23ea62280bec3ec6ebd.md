+   4. Iterate the solution in time with the stencil until we cover the full domain.
+   
+   In explicit FDM, you simply advance and compute the values for the unknown function u. Note that in other forms of FDM (such as implicit FDM), we need to solve a system of equations via a matrix problem. Please refer to (Wilmott et al. 1995) for further details on implicit methods.

# # Example of the FD method

Consider the example where we would like to price a six-month European Call option on Barclays equity (BARC.L). The current equity price of BARC is £75, with a volatility of 30 percent p.a. and a strike of £75. We assume the stock pays no dividends. The current risk-free rate is 5 percent pa. How do we proceed to solve this problem using the FDM?

We know that equity financial derivatives satisfy the Black-Scholes PDE when the stock is modelled using GBM. So we solve the heat diffusion equation we described in the previous section. As we did earlier, we apply the following four phases to solve our FDM problem:

+   1. First discretize the domain.
+   
+   We divide the domain into N space divisions dS and M time divisions dt, thus N=5 and M=4. We first apply these values both in space and time dimensions with time steps \(\Delta\tau, \Delta x\).
+   
+   Thus, we obtain six points in time as follows:

```
\(\tau = \{0.0225, 0.0180, 0.0135, 0.0090, 0.0045, 0.0000\}\)
```

Five points in space is shown as follows:

```
\(x = \{-1, -0.5, 0.0, +0.5, +1\}\)
```