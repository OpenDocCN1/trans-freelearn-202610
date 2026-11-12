The LMM in reality can be understood not as a single model, but rather as a large family of models (Rebonato 1998) and (Brigo and Mercurio 2006). Its many variants include the number of factors considered, the type of volatility modeling used, the type of correlation modeling used, whether stochastic volatility or SABR are used, whether forward LIBOR rates or swap rates are used, and whether semi-analytical or numerical solution methods are used, among others.

Our methodology and notation closely follows that of (Pelsser 2000), which, even though succinct, provides a clear introduction to the LMM. From all the possible variations of the LMM, in this work, we chose the simplest implementation—embodied in the use of lognormal SDEs (GBM) for the forward rates and a single Wiener process driving the volatility in all rates (that is, a one factor case). Under these conditions, we further explore the use of flat volatility.

We first divide the term structure of interest rates \( N \) in a set of forward rates \( L \) and a set of reset times \( T \) as follows:

\( L_1(t), L_2(t), L_3(t),..., L_N(t) \quad \text{and} \quad T_0, T_1, T_2,..., T_N \)

Each of the preceding forward rates will have its own stochastic process driving them, which will result in \( N \) stochastic processes. Following BGM, we use Geometric Brownian Motion (BGM) following (Brace, Gatarek, and Musiela 1997) to describe each of these stochastic processes as follows:

\( dL_i = \mu_i L_i dt + \sigma_i L_i dW_i^Q \quad i = 1, N \)

Equation 8

We now further simplify the model and use a single factor driving all the forward rates. This simplification can be later relaxed into a multifactor LMM. It can be shown that by choosing the last rate as a terminal measure, the drift has the form as follows:

\( \mathrm{dL}_i = - \sum_{k=i+1}^{N} \frac{\alpha_k \sigma_k(T_n) L_k(T_n)}{1 + \alpha_k L_k(T_n)} \Omega_i(t) L_i(T_n) \mathrm{dT} + \Omega_i(T_n) \mathrm{dW} \)

Note that the drift in the preceding GBM equation is a function of the forward rate, thus not constant but state-dependent.