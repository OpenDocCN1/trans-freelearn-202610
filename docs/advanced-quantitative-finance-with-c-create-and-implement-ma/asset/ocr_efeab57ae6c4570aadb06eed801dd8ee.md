![](bbox=[0.26, 0.08, 0.74, 0.31])

Simulated stock prices

# Foreign exchange

In the forex asset class, the underlying is the value of an exchange rate. For example, the current exchange rate between the euro (EUR) and the British pound sterling (GBP) at some particular time. The exchange rate could be EUR/GBP = 1.31, meaning that £1 will be exchanged by € 1.31, and the current time could be 11:33:24 on May 13, 2013.

Thus, in mathematical terms, the exchange rate can be represented as a function X(t), which is a scalar function of time, just like in the case of equities. The exchange rate X(t) is thus modeled as an stochastic variable. In mathematical terms, the behavior of X(t) is described using an SDE just like in the case of equities. However, while for equities we used GBM, in the case of forex, we will use a variation that comes from the work of (Garman-Kohlhagen 1983). According to this model, the stochastic differential equation for exchange rates can be expressed as follows:

```
$dX = (r_d - r_f)Xdt + \sigma X dW^Q$
Equation 4
```

In the preceding equation, rd and rf represent the domestic and the foreign risk-free interest rates. The volatility σ is a parameter calibrated to market-quoted instruments.

As we did earlier, before proceeding for the purposes of pricing derivatives, we require to transform equation 1 from continuous time into discrete time to model the behavior of exchange rates say every day (Δt = 1 day).