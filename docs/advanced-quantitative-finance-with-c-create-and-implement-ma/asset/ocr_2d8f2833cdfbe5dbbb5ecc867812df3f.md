# Credit Derivatives with C++

In this last chapter, we focus on the application of C++ to the pricing of credit derivatives. We consider two examples: the use of the Merton model to price a defaultable firm’s equity plus the firm’s default probability (basic example) and the pricing of Credit Default Swap (CDS) (advanced example). The first example is based on the structural approach to credit risk, while the second is based on the intensity approach. We provide the full working C++ implementation for both the examples. A simpler C implementation (without the OO features) can be found in the accompanying book website.

# Basic example – bankruptcy (CR1)

In this example, we will study the default (bankruptcy) of a firm using the (Merton 1974) model. For more information, see "On the Pricing of Corporate Debt: The Risk Structure of Interest Rates". In this model, the dynamics of the firm are described using Geometric Brownian Motion (GBM). The capital structure of the firm is assumed to be very simple: the firm’s assets (V) composed entirely of equity (E) and debt (D). For a given maturity T, default happens if the firm’s assets at maturity V(T) are less than the value of the debt (D) that the firm has to pay at that time.

In this context, we can study the probability of default of a firm using the Monte Carlo simulation. By generating a number of possible trajectories in which the firm can evolve and counting the times that the firm satisfies the default condition, we can estimate the likelihood of bankruptcy of a firm.