
Counterparty B pays A a single contingent payment at the time of default of underlying C. The amount paid is equal to the notional minus the recovery rate. In mathematical terms, it can be expressed as follows:

$$N(1 - R)$$

Like in an IRS, the "price" of the contract is obtained by computing the present value of each leg (the sum of the expected premium payments called **Premium Leg (PL)** and the sum of the expected default payment called **Default Leg (DL)**). In mathematical terms, PL and DL are expressed as follows:

$$PL = \sum_{i=1}^{N} \pi \times N \times P(T_i) \times \Delta t \times DF_i$$

$$DL = \sum_{i=1}^{N} N \times (1 - R) \times \left[ P(T_{i-1}) - P(T_i) \right] \times DF_i$$

In the preceding equations, P(T) is the survival probability at time t, N is the notional, R is the recovery rate, and DF(t) is the discount factor at time t. For fair pricing, these legs must be equal and with this, we can determine what should be the fair value of the premium paid (also called CDS spread). The value of this spread, denoted by the Greek letter $\pi$, is regarded as the price of the CDS contract. In mathematical terms, it can be expressed as follows:

$$\pi = \frac{ \sum_{i=1}^{N} \pi \times N \times P(T_i) \times \Delta t \times DF_i }{ \sum_{i=1}^{N} N \times (1 - R) \times \left[ P(T_{i-1}) - P(T_i) \right] \times DF_i }$$

The pricing algorithm we present in the Bento Box template for the CDS attempts to compute the premium from the preceding equation.

As we can see, this calculation is deterministic and therefore no Monte Carlo simulation is required. The credit model we illustrate here is an example of "intensity models" based on the work of "Pricing Derivatives on Financial Securities Subject to Credit Risk".

Code snippet 2 illustrates the implementation of this pricing in C++.

We will consider the example where the contract duration is one year, quarterly payments (that is, four payments per year), notional = 100 million USD, risk-free rate = 5 percent pa, hazard rate of underlying = 1 percent pa, recovery rate = 50 percent. For these inputs, the CDS spread is 50.00626 basis points.

In the following figure, we find the Bento Box framework applied to our CDS problem:

![](img/29d9d2cdc85db7e20e91367a93390be2_107_0.png)

Bento Box template for CDS (CR2)

The upcoming code snippets implement the algorithm from the Bento Box template.

### Code 24 - CR2_main.cpp (CDS)

The following is the code snippet for CR2_main.cpp file:

```cpp
// CR2_main.cpp

// It requires CR2_source.cpp
#include "CR2.h"
#include <iostream>

using namespace std;

int main()
{
    cout << "\n *** START CR2: Credit Default Swap *** \n";

    // STEP 1: INPUT PARAMETERS
    auto T = 1.0; // maturity
    auto N = 4; // number of payments per year
    auto notional = 100.0; // notional
    auto r = 0.05; // risk free interest rate
    auto h = 0.01; // hazard rate
    auto rr = 0.50; // recovery rate

    // Construct a CR2 object from the input parameters:
    CR2 cr2(T, N, notional, r, h, rr);

    // Obtain the value of premium from member function
    "get_premium()":

    auto cr2_results =
    cr2.get_pv_premium_and_default_legs_and_cds_spread();

    // STEP 6: OUTPUT RESULTS
    cout << "\n PV premium leg = "
    << cr2_results.pv_premium_leg << "\n";

    cout << "\n PV default leg = "
    << cr2_results.pv_default_leg << " \n";

    cout << "\n cds_spread = "
    << cr2_results.cds_spread_in_bps << " bps \n";

    cout << "\n *** END CR2: Credit Default Swap *** \n";

    return 0;
}
```

### Code 25 - CR2_source.cpp (CDS)

The following is the code snippet for CR2_source.cpp file:

```cpp
// CR2_source.cpp

#include "CR2.H"
#include <vector>
#include <cmath>

using namespace std;

CR2_results CR2::find_pv_premium_and_default_legs_and_cds_spread()
const
{
    auto pv_premium_leg = 0.0; // sum premium leg
    auto pv_default_leg = 0.0; // sum default leg
    auto t = 0.0; // current time
    auto cds_spread = 0.0;
    auto array_size = static_cast<int>(N*T + 1);
    vector <double> DF(array_size);
    vector <double> P(array_size);

    P[0] = 1.0;

    auto dt = T / N;

    // STEP 2: LOOP FOR ALL PAYMENTS
    for (int j = 1; j < array_size; j++)
    {
        t = j*dt;
        DF[j] = exp(-r*t);
        P[j] = exp(-h*t);
    }

    // STEP 3: COMPUTE PREMIUM PAYMENTS
    pv_premium_leg = pv_premium_leg + DF[j] * notional*dt*P[j];

    // STEP 4: COMPUTE DEFAULT PAYMENTS
    pv_default_leg = pv_default_leg + DF[j] * (1.0 -
            rr)*notional*(P[j - 1] - P[j]);
}

// STEP 5: COMPUTE CDS SPREAD
cds_spread = pv_default_leg / pv_premium_leg;

// Composing the CR2_results class:
CR2_results results;
results.pv_premium_leg = pv_premium_leg;
results.pv_default_leg = pv_default_leg;
results.cds_spread_in_bps = cds_spread * 10000;
return results;
}
```

> For code snippet 26 CR2.h, please refer to the code in the code bundle.

To compute the advanced example (CR2), you will have to compile and run code snippets 24, 25, and 26; afterwards, you should obtain the following screenshot:

![](img/29d9d2cdc85db7e20e91367a93390be2_110_0.png)

CDS (CR2) screenshot with results

## Summary

In this chapter, we have solved two pricing problems in credit derivatives. We have seen a basic example (using structural models) and a more advanced one (using intensity models). There are many possible variations and more complex contracts, but these two are the main families that will give you an idea of how to go forward in this fascinating asset class. This concludes our survey of examples implementing different types of financial derivatives in C++.

## C++ Numerical Libraries for Option Pricing

Implementing financial derivatives in C++ could be a complex task. As we have shown in this book, it requires knowledge not only of the mathematical models and numerical methods required for their implementation in the forms of C++ code, but it also requires the use of reliable support mathematical and financial libraries. For example, when you need to obtain random samples from a standard normal distribution or when you need to invert a matrix. In these cases, instead of implementing these algorithms ex novo, what we can do is make use of numerical libraries that exists for this purpose. These contain algorithms that have been used for many years and therefore have been validated by many users before. Using these libraries will significantly accelerate our implementation of advanced pricing models. Some examples of these libraries are mentioned in the upcoming sections.

### Numerical recipes

License: Commercial.

Website: http://www.nr.com.

A collection of widely used and reliable set of C++ numerical routines can be found in the book "Numerical Recipes: The Art of Scientific Computing, 3rd Edition". These set of routines are regarded as the "gold standard" by the top universities and research institutions around the world. There is an excellent associated website that can be found at http://www.nr.com/. The book contains the description of the theoretical background of the routines and it gives access to the C++ code. There are more than 400 C++ numerical routines covering topics, such as Solution of Linear Algebraic Equations, Matrix Algebra, Interpolation and Extrapolation, Integration, and Random Numbers.

### Financial numerical recipes

License: Free/GNU.

Website: http://finance.bi.no/~bernt/gcc_prog/.

This website contains a large number of very useful C++ numerical and financial programs that have been developed by Bernt Arne Odegaard. They follow the ANSI C++ standard and have a large accompanying manual named Circa (250 pages) with the formulas used and the references involved. This library can be found at http://finance.bi.no/~bernt/gcc_prog/.

### The QuantLib project

License: Free/GNU.

Website: http://quantlib.org/.

The QuantLib project is a large project offering software for Quantitative Finance. It has been used for modeling, trading, and risk management in the financial sector. The software is written in C++ and has been subsequently exported to various languages such as C#, Objective Caml, Java, Perl, Python, GNU R, Ruby, and Scheme. QuantLib has many useful tools including yield curve models, solvers, PDEs, Monte Carlo (low-discrepancy), exotic options, VAR, and so on.

### The Boost library

License: Free/GNU.

Website: www.boost.org.

The Boost project offers peer-reviewed portable C++ source libraries that are freely available under GNU GPL. These libraries have been created with the intention of making them useful and usable across a broad spectrum of applications. Ten Boost libraries are included in the C++ Standards Committee's Library Technical Report (TR1) and in the new C++11 Standard. Examples include Accumulators, Array, Chrono, Filesystem, Geometry, Math, Math/Statistical Distributions, and MPI.
