```
V[3] = max(diff, 0.0);
diff = L[3] [3] - K;
V[4] = max(diff, 0.0);
diff = L[4] [4] - K;
V[5] = max(diff, 0.0);

// STEP 7: COMPUTE NUMERAIRE-BASED CAPLETS
Vprime[1] = V[1] * D[1] [0] / D[5] [0];
Vprime[2] = V[2] * D[2] [1] / D[5] [1];
Vprime[3] = V[3] * D[3] [2] / D[5] [2];
Vprime[4] = V[4] * D[4] [3] / D[5] [3];
Vprime[5] = V[5] * D[5] [4] / D[5] [4];

// STEP 8: COMPUTE CAP PAYOFF
Vcap[nsim] = Vprime[1] + Vprime[2] + Vprime[3] + Vprime[4] +
Vprime[5];
}
// end main MC loop

// STEP 9: COMPUTE DISCOUNTED EXPECTED PAYOFF
sumcap = 0.0;

for (int nsim = 0; nsim < M; ++nsim)
{
sumcap += Vcap[nsim];
}

payoff = D[N + 1] [0] * sumcap / M;

IR_results results(Vcap, payoff);

return results;
}
```

We consider the example of an IRS having a cap with a strike 5 percent and a maturity of 2.5 years. We assume a flat term structure of 5 percent with a forward volatility of 15 percent. Payments are every six months and a notional of 1 EUR. The floating rate is EURIBOR6M.