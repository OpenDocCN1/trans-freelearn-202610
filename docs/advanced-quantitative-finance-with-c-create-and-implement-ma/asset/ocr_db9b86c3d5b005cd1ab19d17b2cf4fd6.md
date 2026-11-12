Now, if we consider the credit risk, we have the \( PV \) as follows:

\( PV = \exp\big(-(r + \lambda)T\big) \times FV \)

\( PV = \exp\big(-(0.05 + 0.03)(1)\big) \times (1,000,000) = 880,000 \)

Note that the second cash flow is smaller than the first. The effect of the credit risk is to view the value of the cash flow today (its \( PV \)) as reduced because not being certain, it has to be multiplied by the chance that it happens (that is, its survival probability).

In the previous analysis, we have assumed that when default occurs, all of the future cash flow \( FV \) will be lost. However, in most real situations, some fraction of money is recovered. If we define this fraction as the recovery rate \( R \), the present value \( PV \) of the risky future cash flow is still the same, as follows:

\( PV = \exp(-rT) \times E \big[ FV \big] \)

But the expectation is now resolved in terms of two possible states, default or no default, and each of them is multiplied by its respective probabilities, as follows:

\( E \big[ FV \big] = \underbrace{FV \times P(0,T)}_{\text{NO DEFAULT}} + \underbrace{FV \times R \times (1 - P(0,T))}_{\text{DEFAULT}} \)

In the preceding equation, we have taken into account the recovery rate \( R \) in the case of default. The present value of the risky cash flow \( PV \) is now as follows:

\( PV = \exp(-rT) \times FV \times \big( P(0,T) + R(1 - P(0,T)) \big) \)

# Summary

This chapter gave an overview of the fundamental models used to price derivatives in the modern financial markets. We will now take these models as basis to model the underlying of the various asset classes we reviewed and apply a number of numerical methods to implement their calculations efficiently in a computer.

In the next chapter, we will concentrate on numerical methods.