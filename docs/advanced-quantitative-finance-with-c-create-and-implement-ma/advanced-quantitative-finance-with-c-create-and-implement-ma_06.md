## 5 使用 C++ 的外汇衍生品

我们现在进入货币或外汇衍生品的世界，并了解如何使用 C++ 对其进行定价。我们考虑两个示例：欧洲看涨期权的定价（基本示例）和向外敲出障碍看涨期权的定价（高级示例）。我们为两者提供了完整可用的 C++ 实现。我们遵循在 "Foreign Currency Option Values" 中找到的外汇货币演变模型。一个更简单的 C 实现（不带 OO 功能）可在本章的代码包中找到。如果您是 OOP 新手，建议您先学习 C 实现，然后再学习 C++ 实现。

### 基本示例 – 欧洲外汇看涨期权 (FX1)

在此示例中，我们演示外汇普通欧式看涨期权的定价。我们的目标是计算这种金融衍生品的权利金。

合同的完整细节，包括数学模型及其数值方法的选择，显示在以下欧洲外汇看涨期权 (FX1) 的 Bento Box 模板中：

![](img/29d9d2cdc85db7e20e91367a93390be2_72_0.png)

欧洲外汇看涨期权 (FX1) 的 Bento Box 模板

我们按顺时针方向完成 Bento Box 的内容，从左上角开始。具体步骤如下：

- 1. 衍生品合约：我们首先填写合约的所有数据，特别是收益函数，在本例中如下：

> H(X_T) = max(X_T - K, 0)
> 方程 1

- 2. 数学模型：我们应为标的资产选择数学模型，在货币的情况下，该模型为 Garman-Kohlhagen 模型。
- 3. **数值方法**：我们选择要使用的数值方法，在此例中，我们选择有限差分法。
- 4. **算法**：我们构建将这些计算作为一系列计算步骤组合在一起的算法，这将作为在 C++ 中实现的蓝图。

请注意，与蒙特卡洛模拟相比，有限差分算法在运行时不需要随机数生成器。所有计算都是确定性的。

有限差分方法的一个重要特征是它们需要定义一个网格。该网格基本上是将要近似 **偏微分方程 (PDE)** 的坐标集合。例如，在股票的情况下，Black-Scholes PDE 由两个自变量定义：股票价格 S 和时间 t。在货币的情况下，Garman-Kohlhagen PDE 由两个变量定义：汇率 X 和时间 t。因此，求解域是 X 和 t 平面中 X 和 t 所能取的所有可能值对所定义的区域。例如，如果我们考虑一个行权价为 1.0 EUR/GBP、现价为 1.0 EUR/GBP 且期限为一年的欧洲货币看涨期权，我们的求解域 gamma 可以定义为 X 在 0.5 到 1.5 之间、t 在 0 到 1 之间的值范围。在数学上，这可以表示如下：

```
\Omega = \{(X, t)\forall X \in [0.5, 1.5] \times t \in [0, 1]\}
```

方程 2

然后需要对这个矩形域进行划分或离散化。这意味着我们必须将其从连续域转换为离散域。通常在有限差分法中，我们将其沿 X 轴划分为 N 个等距步长，沿 t 轴划分为 M 个等距步长。结果是一个类似网格的网，这也是名称的由来。

请注意，我们介绍了第 3 章“数值方法”中描述的显式有限差分法的实现，并使用了变量变换。这样做是为了将原始 PDE 转换为等效但简化的无量纲 PDE，用于描述热传导。这个无量纲版本的 PDE 使用 FDM 求解更容易。

由于此变换，求解域未转换为两个新变量 x 和 $\mathcal{T}$。因此，PDE 在由以下方程定义的域中求解：

```
\Omega = \{(x, \tau)\}
```

方程 3





我们考虑一个货币欧洲看涨期权的示例，行权价为 0.75 EUR/USD，现货价格为 0.75 EUR/USD。该期权还有六个月到期。我们将 x 轴分为 N=5 步，将 tau 轴分为 M=6 步。在这些条件下，期权权利金为 4.36 EUR/USD。

即将展示的代码片段实现了来自 Bento Box 模板的算法。

### Code 9 – FX1_main.cpp（有限差分 FX 欧洲看涨期权）

以下是 FX1_main.cpp 文件的代码片段：

```cpp
// FX1_main.cpp
// requires FX_source.cpp, FX_print.cpp

#include "FX.h"

using namespace std;

int main()
{
    cout << "\n *** START FX1: Finite Difference European Call *** \n\n";

    // STEP 1: INPUT PARAMETERS
    auto T = 0.5; // maturity
    auto K = 75.0; // strike
    auto S0 = 75.0; // spot
    auto sigma = 0.30; // volatility
    auto r = 0.05; // interest rate
    auto dx = 0.5; // space step
    auto dt = 0.1; // time step
    auto N = 5; // number of space steps
    auto M = 6; // number of time steps

    // Construct a FX_EQ1 object from the input parameters:
    FX fx_eq1(T, K, S0, sigma, r, dt, dx, N, M);

    // Ask the object to evaluate the FX data for European Call:
    auto result = fx_eq1.get_data_and_premium();

    // STEP 7: OUTPUT RESULTS
    cout << result;

    cout << "\n *** END FX1: Finite Difference European Call ***\n";

    return 0;
}
```

### Code 10 - FX1_source.cpp（有限差分 FX 欧洲看涨期权）

以下是 FX1_source.cpp 文件的代码片段：

```cpp
// FX1_source.cpp
#include "FX.h"
#include "matrix.h"
#include <algorithm>
using namespace std;

result_data FX::evaluate_data_and_premium() const
{
   double dtau, alpha, k;

   vector<double> t, tau, S, x;

   matrix<double> u, v;

   matrix_resize(u, N, M);

   matrix_resize(v, N, M);

   // Therefore, both the matrices u, v are resized to N by M
   // Now, let us resize the vectors t, tau, S and x:
   t.resize(M);
   tau.resize(M);
   S.resize(N);
   x.resize(N);

   dtau = dt * (0.5*sigma*sigma);
   alpha = dtau / (dx*dx);
   k = r / (0.5*sigma*sigma);
   double xmin = -1;
   double xmax = +1;

   // STEP 2: SETUP MESH (x and tau grids)
   for (int i = 0; i < N; i++)
   {
       x[i] = xmin + i*dx;
       S[i] = K*exp(x[i]);
   }

   for (int j = 0; j < M; j++)
   {
       t[j] = j*dt;
       tau[j] = (T - t[j]) / (0.5*sigma*sigma);
   }

   // STEP 3: SETUP INITIAL CONDITION
   for (int i = 0; i < N; i++)
   {
       u[i][0] = max(exp(0.5*(k + 1)*x[i]) - exp(0.5*(k - 1)*x[i]),
       0.0);
   }

   // STEP 4: SETUP BOUNDARY CONDITIONS
   for (int j = 1; j < M; j++)
   {
       u[0][j] = 0.0;
       u[N - 1][j] = u[N - 1][0];
   }

   // STEP 5: COMPUTE FORWARD DIFFERENCES
   for (int j = 0; j < M - 1; j++)
   {
       for (int i = 1; i < N - 1; i++)
       {
           u[i][j + 1] = alpha*u[i + 1][j] + (1 - 2 * alpha)*u[i][j] +
           alpha*u[i - 1][j];
       }
   }

   // STEP 6: TRANSFORM SOLUTION FROM X TO S COORDINATES (u and v)
   for (int j = 0; j < M; j++)
   {
       for (int i = 0; i < N; i++)
       {
           v[i][j] = pow(K, (0.5*(1 + k)))*pow(S[i] , (0.5*(1 - k)))*exp(1.0 / 8.0*(k + 1)*(k + 1)*sigma*sigma* (T - t[i]))*u[i][j];
       }
   }
   result_data result(alpha, dtau, k, x, S, t, tau, u, v);
   return result;
}
```

有关代码片段 11 FX.h、代码片段 12 FX_print.cpp 和代码片段 13 matrix.h，请参阅本书的代码包。

要计算基本示例（FX1），需要编译并运行代码片段 9、10、11、12 和 13（其中包含矩阵和打印实用程序）；之后，您应该获得以下截图：

![](img/29d9d2cdc85db7e20e91367a93390be2_77_0.png)

基本示例（FX1）：带结果的 FX 欧洲看涨期权截图

### 高级示例 – FX 障碍期权（FX2）

在此第二个示例中，我们考虑一种奇异期权的定价：一种具有看涨期权收益的向上敲出障碍期权。该方法的详细内容显示在下面的 FX 障碍期权 Up and Out（FX2）Bento Box 模板中：

![](img/29d9d2cdc85db7e20e91367a93390be2_78_0.png)

FX 障碍期权 Up and Out（FX2）选项的 Bento Box 模板

需要注意的是，在定价连续监控的障碍期权时，与蒙特卡洛（MC）相比，使用有限差分方法（FDM）具有很大优势。这是因为 MC 很难融入连续监控特征，除了在 MC 程序中增加修复/观察点外，我们几乎没有其他选择。然而，这将显著增加 MC 的计算时间。在 FDM 中我们不需要这样做，因此它更高效。

我们的目标是像之前一样计算期权权利金。

向上敲出障碍期权与标准欧洲看涨期权非常相似，但有一个关键区别——如果标的资产突破限制的上障碍，期权价值为零。因此，定价算法及其实现几乎相同，但区别在于上边界条件现在将值设为零。

障碍期权在金融中很有用，因为其权利金小于标准欧洲期权。它们对投资者来说更便宜，因为投资者承担了如果标的资产水平过高（向上敲出障碍）或过低（向下敲出障碍）而不执行期权的风险。

我们考虑与之前相同的示例，但障碍 B = 1.5 EUR/USD。在这些条件下，该期权的权利金为 4.11 EUR/USD，执行时间为 2.22 秒。

即将展示的代码片段实现了来自 Bento Box 模板的算法。

### Code 14 - FX2_main.cpp（FDM FX 障碍期权）

以下是 FX2_main.cpp 文件的代码片段：

```cpp
// FX2_main.cpp
// requires FX2_source.cpp, FX_print.cpp

#include "FX.h"
#include <iostream>

using namespace std;

int main()
{
    cout << "\n *** START FX2: Finite Difference"
         << " European Up-and-Out Barrier Call *** \n\n";

    // STEP 1: INPUT PARAMETERS
    auto T = 0.5; // maturity
    auto K = 75.0; // strike
    auto S0 = 75.0; // spot
    auto sigma = 0.30; // volatility
    auto r = 0.05; // interest rate
    auto dx = 0.5; // space step
    auto dt = 0.1; // time step
    auto N = 5; // number of space steps
    auto M = 6; // number of time steps
    // Construct a FX object from the input parameters:
    FX fx_eq2(T, K, S0, sigma, r, dt, dx, N, M);
    // Ask the object to evaluate the FX data
    // for European Up-and-Out Barrier Call:
    auto result = fx_eq2.get_data_and_premium();
    // STEP 7: OUTPUT RESULTS
    cout << result;
    cout << "\n** END FX2: Finite Difference"
    << " European Up-and-Out Barrier Call **\n";
    return 0;
}
```

### Code 15 - FX2_source.cpp（FDM FX 障碍期权）

以下是 FX2_source.cpp 文件的代码片段：

```cpp
// FX2_source.cpp
#include "FX.h"
#include "matrix.h"
#include <algorithm>
using namespace std;
result_data FX::evaluate_data_and_premium() const
{
double dtau, alpha, k;
vector<double> t, tau, S, x;
matrix<double> u, v;
auto resz = [this] (matrix<double>& u, int N, int M) {
// to make number of rows =
u.resize(N);
// to make number of columns =
for (auto& row : u)
row.resize(M);
};
resz(u, N, M);

resz(v, N, M);

// Therefore, both the matrices u, v are resized to N by M
// Now, let us resize the vectors t, tau, S and x:
t.resize(M);
tau.resize(M);
S.resize(N);
x.resize(N);

dtau = dt * (0.5*sigma*sigma);
alpha = dtau / (dx*dx);
k = r / (0.5*sigma*sigma);

double xmin = -1;
double xmax = +1;

// STEP 2: SETUP MESH (x and tau grids)
for (int i = 0; i < N; i++)
{
    x[i] = xmin + i*dx;
    S[i] = K*exp(x[i]);
}

for (int j = 0; j < M; j++)
{
    t[j] = j*dt;
    tau[j] = (T - t[j]) / (0.5*sigma*sigma);
}

// STEP 3: SETUP INITIAL CONDITION
for (int i = 0; i < N; i++)
{
    u[i][0] = max(exp(0.5*(k + 1)*x[i]) - exp(0.5*(k - 1)*x[i]),
                 0.0);
}

// STEP 4: SETUP BOUNDARY CONDITIONS
for (int j = 1; j < M; j++)
{
    u[0][j] = 0.0;
    u[N - 1][j] = 0.0;
}

// STEP 5: COMPUTE FORWARD DIFFERENCES
for (int j = 0; j < M - 1; j++)
{
    for (int i = 1; i < N - 1; i++)
    {



```
u[i][j + 1] = alpha*u[i + 1][j] + (1 - 2 * alpha)*u[i][j] +
                      alpha*u[i - 1][j];
        }
```

```cpp
// STEP 6: TRANSFORM SOLUTION FROM X TO S COORDINATES (u and v)
for (int j = 0; j < M; j++)
{
    for (int i = 0; i < N; i++)
    {
        v[i][j] = pow(K, (0.5*(1 + k)))*pow(S[i], (0.5*(1 - k)))
                  *exp(1.0 / 8.0*(k + 1)*(k + 1)*sigma*sigma* (T -
                  t[i]))*u[i][j];
    }
}
```

```cpp
result_data result(alpha, dtau, k, x, S, t, tau, u, v);

return result;
}
```

要计算高级示例 (FX2)，您需要编译并运行代码片段 14 和 15 以及之前的 11、12 和 13；之后，您应该获得以下截图：

![](img/29d9d2cdc85db7e20e91367a93390be2_82_0.png)

高级示例 (FX2)：FX 向上敲出障碍看涨期权结果截图

### 摘要

在本章中，我们解决了外汇衍生品的两个定价问题。我们看到了一个基本示例和一个更复杂的示例（普通香草）以及一个高级示例（异型），其中包括障碍期权。对于每个示例，我们都提供了完整的 C++ 实现。

我们现在将进入下一个资产类别和利率衍生品。
