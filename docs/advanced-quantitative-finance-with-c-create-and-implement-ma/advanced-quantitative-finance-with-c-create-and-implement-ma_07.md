## 使用 C++ 的利率衍生品

本章阐述了 C++ 在利率衍生品定价中的应用。我们将考虑两个示例：普通香草**利率互换（IRS）**（基本示例）的定价和上限期权（Cap）（高级示例）的定价。我们为两者都提供了完整的可运行 C++ 实现。两个示例均使用单因子**利率市场模型（LMM）**和蒙特卡洛模拟求解。更简单的 C 实现（不包含 OO 特性）可在随书网站上找到。LMM 在《利率动态的市场模型》中有描述。蒙特卡洛模拟的出色描述可在《利率衍生品定价的高效方法》中找到。

### 基本示例 – 普通香草 IRS (IR1)

在此示例中，我们将演示普通香草 IRS 的定价。合约的完整细节，包括数学模型及其数值方法的选择，将在以下便当盒模板中展示：

![](img/29d9d2cdc85db7e20e91367a93390be2_86_0.png)

基本示例（IR1）的便当盒模板

我们的目标是计算该 IRS 的净现值，特别是支付固定利率并接收浮动利率的 IRS。在该合约中，持有人定期支付固定利率并接收浮动利率。

我们将按顺时针方向填写便当盒的内容，从左上角开始。首先，我们将填写合约的所有数据，特别是收益函数，在我们的例子中如下：

$PV = \sum_{i=0}^N Notional \times DF_i \times \alpha \times (L_i - K)$

公式 1

IRS 的现值是该 IRS 未来折现支付的总和。作为支付方 IRS，我们支付固定利率 K 并接收未来的浮动利率 L。该利率在期初确定（即锁定），并一直持续到到期日（进行支付时）。\(DF_i\) 代表相应的折现因子。每笔支付都乘以名义本金和日计数分数 \(\alpha\)。

其次，我们应该为基础工具选择数学模型。在利率方面，可以在短利率模型（如 Vasicek 模型和 Hull-White 模型）或市场模型（如 LMM）之间进行选择。在本章中，我们选择 LMM 来解决这些问题。第三，我们选择要使用的数值方法，在此情况下，我们选择蒙特卡洛方法。该方法将允许我们模拟远期利率的随机行为。第四，我们构建将这些计算作为一系列计算步骤组合在一起的算法，这将作为在 C++ 中实现它的蓝图。

LMM 有许多变体，例如所使用的利率数量（多因子）或所使用的基础工具。（互换 LMM 使用互换利率而非远期利率作为基本未知数。）在本章中，我们将仅考虑单因子（对数正态远期）LMM。

算法在便当盒模板的第 4 个方框中显示。算法的 C++ 实现显示在代码片段 16、17 和 18 中。代码片段 16 是主代码块，代码片段 17 是其关联源文件，而代码片段 18 包含头文件。该算法由 10 个步骤组成，将我们从输入参数（步骤 1）带到 IRS 的现值输出（步骤 10）。

请注意，蒙特卡洛模拟需要随机数生成器才能运行。我们将利用在第 3 章“数值方法”中开发的随机数生成器来定价股票衍生品（Box-Muller 算法）。

我们考虑一个名义本金为一百万欧元的普通香草 IRS 示例。合约期限为一年，支付频率为每三个月一次。因此，浮动利率与 EURIBOR3M 挂钩。固定利率为每年 4%。

我们使用带有蒙特卡洛模拟的 LMM，进行 10,000 次模拟。我们假设初始利率期限结构为平坦的，年利率为 5%。我们还假设远期利率的波动率为 15%（此值通常从市场上观察到的互换期权中校准）。

即将显示的代码片段实现了便当盒模板中的算法。

### 代码 16 – IR1_main.cpp（使用蒙特卡洛 LMM 的 IRS）

以下是 IR1_main.cpp 文件的代码片段：

```cpp
// IR1_main.cpp
// requires random.cpp IR1_source.cpp

#include "IR.h"
#include <iostream>
using namespace std;

int main()
{
    cout << "\n *** START IR1: IRS Monte Carlo Libor Market Model 1F\n * ** \n\n";
    // Plain Vanilla IRS, pays fixed, receives floating
    // freq payments every 3M, maturity 1 year

    // STEP 1: INPUT PARAMETERS
    double notional = 1000000; // notional
    double K = 0.04; // fixed rate IRS
    double alpha = 0.25; // daycount factor
    double sigma = 0.15; // fwd rates volatility
    double dT = 0.25;
    int N = 3; // number forward rates
    int M = 1000; // number of simulations

    // Construct a IR object from the input parameters:
    IR irl(notional, K, alpha, sigma, dT, N, M);

    // Obtain the value of premium from member function
    // get_premium():
    auto results = irl.get_simulation_data();

    // STEP 10: OUTPUT RESULTS
    auto sz = results.datapoints.size();
    for (decltype(sz) nsim = 0; nsim < sz; ++nsim)
    {
        cout << "simIRS[" << nsim << "] = " <<
            results.datapoints[nsim] << endl;
    }

    cout << "\n*** IRS PV = " << results.Value << endl;
    cout << "\n*** END IR1: IRS Monte Carlo Libor Market Model 1F\n*** \n";

    return 0;
}
```

### 代码 17 - IR1_source.cpp（使用蒙特卡洛 LMM 的 IRS）

以下是 IR1_source.cpp 文件的代码片段：

```cpp
// IR1_source.cpp

#include "IR.h"
#include "random.h"
#include "matrix.h"
#include <algorithm>
#include <iostream>

using namespace std;

IR_results IR::run_LIBOR_simulations() const
{
    matrix<double> L; // forward rates
    matrix_resize(L, N + 1, N + 1);
    matrix<double> D; // discount factors
    matrix_resize(D, N + 2, N + 2);
    vector<double> dW(N + 1); // discount factors
    vector<double> FV(N + 2); // future value payment
    vector<double> FVprime(N + 2); // numeraire-rebased FV payment
    vector<double> V(M); // simulation payoff

    // Composing the SampleBoxMuller class:

    SampleBoxMuller normal;

    double df_prod = 1.0;
    double drift_sum = 0.0;
    double sumPV = 0.0;
    double PV = 0.0;

    // STEP 2: INITIALISE SPOT RATES
    L[0][0] = 0.05;
    L[1][0] = 0.05;
    L[2][0] = 0.05;
    L[3][0] = 0.05;

    // start main MC loop
    for (int nsim = 0; nsim < M; ++nsim)
    {
        // STEP 3: BROWNIAN MOTION INCREMENTS
        dW[1] = sqrt(dt)*normal();
        dW[2] = sqrt(dt)*normal();
        dW[3] = sqrt(dt)*normal();
        // STEP 4: COMPUTE FORWARD RATES TABLEAU
        for (int n = 0; n < N; ++n)
        {
            for (int i = n + 1; i < N + 1; ++i)
            {
                drift_sum = 0.0;
                for (int k = i + 1; k < N + 1; ++k)
                {
                    drift_sum += (alpha*sigma*L[k][n]) / (1 + alpha*L[k][n]);
                }
                L[i][n + 1] = L[i][n] * exp((-drift_sum*sigma - 0.5*sigma*sigma)*dt + sigma*dW[n + 1]); // cout <<"L: i= " << i <<", n+1 = " << n+1 " << L[i][n+1] << "\n";
            }
        }
    }

    // STEP 5: COMPUTE DISCOUNT RATES TABLEAU
    for (int n = 0; n < N + 1; ++n)
    {
        for (int i = n + 1; i < N + 2; ++i)
        {
            df_prod = 1.0;
            for (int k = n; k < i; k++)
            {
                df_prod *= 1 / (1 + alpha*L[k][n]);
            }
            D[i][n] = df_prod;
            // cout <<"D: i = " << i <<", n = " << n <<", D[i][n] = " << D[i][n] << "\n";
        }
    }
}
```




// STEP 6: COMPUTE EFFECTIVE FV PAYMENTS
FV[1] = notional*alpha*(L[0][0] - K);
FV[2] = notional*alpha*(L[1][1] - K);
FV[3] = notional*alpha*(L[2][2] - K);
FV[4] = notional*alpha*(L[3][3] - K);

// STEP 7: COMPUTE NUMERAIRE-REBASED PAYMENT
FVprime[1] = FV[1] * D[1][0] / D[4][0];
FVprime[2] = FV[2] * D[2][1] / D[4][1];
FVprime[3] = FV[3] * D[3][2] / D[4][2];
FVprime[4] = FV[4] * D[4][3] / D[4][3];

// STEP 8: COMPUTE IRS NPV
V[nsim] = FVprime[1] * D[1][0] + FVprime[2] * D[2][0] +
FVprime[3] * D[3][0] + FVprime[4] * D[4][0];
}
// end main MC loop

// STEP 9: COMPUTE DISCOUNTED EXPECTED PAYOFF
sumPV = 0.0;
for (int nsim = 0; nsim < M; nsim++)
{
sumPV += V[nsim];
}

PV = sumPV / M;

IR_results results(V, PV);

return results;

关于代码片段 18 IR.h，请参考代码包中的代码。

要计算基本示例 (IR1)，您需要编译并运行代码片段 16、17、18、4、5 和 13（这些片段包含头文件、矩阵和随机函数）。之后，您应该获得以下屏幕：

![](img/29d9d2cdc85db7e20e91367a93390be2_92_0.png)

基本示例 (IR1) 结果屏幕截图

### 高级示例 – 带上限的 IRS (IR2)

在这个第二个示例中，我们考虑带上限的 IRS 的定价。该方法的详细信息显示在以下 Bento Box 模板高级示例中：

### 衍生品合约

### 带上限的 IRS
合约期限 2.5 年。
每 6 个月付款一次。
名义本金 1 欧元。行权价 5%。
浮动利率 EURIBOR6M。
期限结构平坦，为 5%。
波动率
\(PV = \sum_{i=1}^{i} Notional \times DF_i \times \alpha \times max(L_i - K, 0)\)

### 数学模型

根据利率市场模型，浮动利率可以使用几何布朗运动 (GBM) 进行建模：
\(dL_i = \mu L_i dt + \sigma L_i dW\)

### 数值方法

使用蒙特卡洛模拟作为利用佩斯勒表格计算远期利率值的方法。

- 算法
  - 步骤 1：输入参数
  - 步骤 2：初始化即期利率
  - 步骤 3：布朗运动增量
    - for i=1:M (模拟)
  - 步骤 4：计算远期利率
  - 步骤 5：计算折现因子
  - 步骤 6：计算有效 FV 利率
  - 步骤 7：计算重新基准的支付
  - 步骤 8：计算上限收益
    - end
  - 步骤 9：计算折现预期收益
  - 步骤 10：输出结果

### Bento Box 模板

高级示例 (IR2) 的 Bento Box 模板

我们的目标是像之前一样计算互换的净现值。

带上限的 IRS 就像标准 IRS，但有一个关键区别——在每个付款日期，我们计算 (a) 浮动利率与行权价之间的差额和 (b) 零之间的最大值。利用这个差额，我们计算上限期权（caplet）的价值；上限（cap）只是 IRS 中包含的上限期权之和。

该算法在 Bento Box 模板的框 4 中显示。该算法在 C++ 中的实现显示在代码片段 19 和 20 中。代码片段 19 是主代码块，而代码片段 20 是其关联源文件。

### 代码 19 – IR2_main.cpp (带蒙特卡洛 LMM 的上限)

以下是 IR2_main.cpp 文件的代码片段：

```cpp
// IR2_main.cpp
// requires random.cpp IR2_source.cpp

#include "IR.h"
#include <iostream>
using namespace std;

int main()
{
    std::cout << "\n *** START IR2: CAP Monte Carlo Libor Market Model 1F * * \n\n";
    // STEP 1: INPUT PARAMETERS
    double K = 0.05; // strike caplet
    double alpha = 0.5; // daycount factor
    double sigma = 0.15; // fwd rates volatility
    double dT = 0.5;
    int N = 4; // number forward rates
    int M = 1000; // number of simulations

    // Construct a IR object from the input parameters:
    IR ir2(K, alpha, sigma, dT, N, M);

    // Obtain the value of premium from member function
    // "get_premium()":

    auto results = ir2.get_simulation_data();

    // STEP 10: OUTPUT RESULTS
    auto sz = results.datapoints.size();

    for (decltype(sz) nsim = 0; nsim < sz; ++nsim)
    {
        cout << "Vcap[" << nsim << "] = " << results.datapoints[nsim]
        << endl;
    }

    cout << "\n *** IRS cap = " << results.Value << "\n";

    cout << "\n *** END IR2: CAP Monte Carlo Libor Market Model 1F * * \n";

    return 0;
}
```

### 代码 20 – IR2_source.cpp (带蒙特卡洛 LMM 的上限)

以下是 IR2_source.cpp 文件的代码片段：

```cpp
// IR2_source.cpp

#include "IR.h"
#include "random.h"
#include "matrix.h"
#include <algorithm>
#include <iostream>

using namespace std;

IR_results IR::run_LIBOR_simulations() const
{
    matrix<double> L; // forward rates
    matrix_resize(L, N + 1, N + 1);
    matrix<double> D; // discount factors
    matrix_resize(D, N + 2, N + 2);
    vector<double> dW(N + 1); // discount factors
    vector<double> V(N + 2); // caplet payoff
    vector<double> Vprime(N + 2); // numeraire-rebased caplet payoff
    vector<double> Vcap(M); // simulation payoff

    // Composing the SampleBoxMuller class:
    SampleBoxMuller normal;

    double df_prod = 1.0;
    double drift_sum = 0.0;
    double sumcap = 0.0;
    double payoff = 0.0;

    // STEP 2: INITIALISE SPOT RATES
    L[0][0] = 0.05;
    L[1][0] = 0.05;
    L[2][0] = 0.05;
    L[3][0] = 0.05;
    L[4][0] = 0.05;

    // start main MC loop
    for (int nsim = 0; nsim < M; ++nsim)
    {
    // STEP 3: BROWNIAN MOTION INCREMENTS
    dW[1] = sqrt(dt) * (normal());
    dW[2] = sqrt(dt) * (normal());
    dW[3] = sqrt(dt) * (normal());
    dW[4] = sqrt(dt) * (normal());

    // STEP 4: COMPUTE FORWARD RATES TABLEAU
    for (int n = 0; n < N; ++n)
    {
       for (int i = n + 1; i < N + 1; ++i)
       {
          drift_sum = 0.0;
          for (int k = i + 1; k < N + 1; ++k)
          {
             drift_sum += (alpha*sigma*L[k][n]) / (1 + alpha*L[k][n]);
          }
          L[i][n + 1] = L[i][n] * exp((-drift_sum*sigma - 0.5*sigma*sigma)*dt + sigma*dW[n + 1]);
          // cout <<"L: i = " << i <<"", n+1 = " << n+1 <<"", = " << L[i][n+1] << " " \n";
       }
    }

    // STEP 5: COMPUTE DISCOUNT RATES TABLEAU
    for (int n = 0; n < N + 1; ++n)
    {
       for (int i = n + 1; i < N + 2; ++i)
       {
          df_prod = 1.0;
          for (int k = n; k < i; k++)
          {
             df_prod *= 1 / (1 + alpha*L[k][n]);
          }
          D[i][n] = df_prod;
          // cout <<"D: i = " << i <<"", n = " << n <<"", D[i][n] = " << D[i][n] << " \n";
       }
    }

    // STEP 6: COMPUTE CAPLETS
    double diff;
    diff = L[0][0] - K;
    V[1] = max(diff, 0.0);
    diff = L[1][1] - K;
    V[2] = max(diff, 0.0);
    diff = L[2][2] - K;
    V[3] = max(diff, 0.0);
    diff = L[3][3] - K;
    V[4] = max(diff, 0.0);
    diff = L[4][4] - K;
    V[5] = max(diff, 0.0);

    // STEP 7: COMPUTE NUMERAIRE-BASED CAPLETS
    Vprime[1] = V[1] * D[1][0] / D[5][0];
    Vprime[2] = V[2] * D[2][1] / D[5][1];
    Vprime[3] = V[3] * D[3][2] / D[5][2];
    Vprime[4] = V[4] * D[4][3] / D[5][3];
    Vprime[5] = V[5] * D[5][4] / D[5][4];

    // STEP 8: COMPUTE CAP PAYOFF
    Vcap[nsim] = Vprime[1] + Vprime[2] + Vprime[3] + Vprime[4] + Vprime[5];
    }
    // end main MC loop

    // STEP 9: COMPUTE DISCOUNTED EXPECTED PAYOFF
    sumcap = 0.0;

    for (int nsim = 0; nsim < M; ++nsim)
    {
    sumcap += Vcap[nsim];
    }

    payoff = D[N + 1][0] * sumcap / M;

    IR_results results(Vcap, payoff);

    return results;
}
```

我们考虑一个具有 5% 行权价和 2.5 年期限的带上限 IRS 示例。我们假设期限结构为 5%，远期波动率为 15%。付款每六个月一次，名义本金为 1 欧元。浮动利率为 EURIBOR6M。

要计算高级示例 (IR2)，您需要编译并运行代码片段 19、20、18、4、5 和 13（这些片段包含头文件、矩阵和随机函数）。之后，您应该获得以下屏幕截图：

![](img/29d9d2cdc85db7e20e91367a93390be2_98_0.png)

高级示例 (IR2) 结果屏幕截图

### 摘要

在本章中，我们解决了利率衍生品的两个定价问题。我们已经看到了一个基本示例（普通香草 IRS）和一个高级示例。对于每个示例，我们都提供了完整的 C++ 实现。

我们将在下一章中继续讨论最后一类资产，即信用衍生品。




