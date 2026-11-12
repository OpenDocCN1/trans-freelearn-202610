
我们可以按照下方截图中的表格排列算法的数值结果，使用转换后的变量（上表）或原始变量（下表），其中可以发现，当 S=75 且 t=0 时，期权价格为 £4.20：

| x/tau | -1 | -0.5 | 0 | 0.5 | 1 |
|---|---|---|---|---|---|
| 0.0225 | 0.00 | 0.00 | 0.06 | 0.71 | 1.82 |
| 0.0180 | 0.00 | 0.00 | 0.05 | 0.70 | 1.82 |
| 0.0135 | 0.00 | 0.00 | 0.04 | 0.69 | 1.82 |
| 0.0090 | 0.00 | 0.00 | 0.02 | 0.68 | 1.82 |
| 0.0045 | 0.00 | 0.00 | 0.01 | 0.68 | 1.82 |
| 0.0000 | 0.00 | 0.00 | 0.00 | 0.67 | 1.82 |

| S/t | 27.59 | 45.49 | 75.00 | 123.65 | 203.87 |
|---|---|---|---|---|---|
| 0.0 | 0.00 | 0.15 | 4.20 | 50.49 | 125.93 |
| 0.1 | 0.00 | 0.09 | 3.41 | 50.14 | 126.56 |
| 0.2 | 0.00 | 0.05 | 2.60 | 49.79 | 127.20 |
| 0.3 | 0.00 | 0.02 | 1.76 | 49.42 | 127.84 |
| 0.4 | 0.00 | 0.00 | 0.90 | 49.04 | 128.48 |
| 0.5 | 0.00 | 0.00 | 0.00 | 48.65 | 128.87 |

有限差分定价示例。

## 摘要

在本章中，我们回顾了当前用于定价金融衍生品的三种关键数值方法的基础知识。对于每种方法，我们都提供了算法和数值示例。此外，这些方法的更高级特性可以参考先前讨论的所有章节中提到的优秀教材：(Glasserman 2003)、(Kloeden and Platen 1992) 和 (Wilmott et al. 1995)。

并非所有方法在所有情况下都适用，就像工具箱里的工具一样。某些方法对解决某些特定问题更有效。例如，使用二叉树，评估美式期权也很简单，而对于蒙特卡洛方法，则不那么直接。蒙特卡洛方法在高维问题中更为强大，而有限差分法可以有效用于低维问题。

## C++ 中的股票衍生品

在前两章中，我们描述了用于模拟金融衍生品标的资产行为的关键数学模型（第 2 章，数学模型）以及用于对其定价的主要数值方法（第 3 章，数值方法）。

在本章中，我们将这些要素应用于股票衍生品的定价。我们考虑两个示例：标准欧式看涨期权定价（基本示例）以及基于两个资产最大值的股票篮子定价（高级示例）。我们为两者提供了完整的可运行 C++ 实现。注意，如果你是 OOP 新手，建议先学习 C 语言实现，再学习 C++ 实现，两者均可在本章的代码包中获取。

### 基本示例 – 欧式看涨期权

在本例中，我们考虑标准欧式看涨期权的定价。这个示例虽然非常简单，但至关重要；它将作为用蒙特卡洛模拟求解的其余期权定价问题的基础模块。

合同的完整特征、数学模型的选择及其数值方法如下所示，采用便当盒模板展示：

![](img/29d9d2cdc85db7e20e91367a93390be2_62_0.png)

基本示例的便当盒模板：欧式看涨期权

我们的目标是计算该金融衍生品的权利金。我们按顺时针方向填写便当盒的内容，从左上角开始。我们首先填写合同的所有数据，特别是支付函数，对于简单的欧式看涨期权，该函数如下：

$H(S_T) = \max(S_T - K, 0)$

其次，我们应为标的资产选择数学模型，在股票的情况下为 GBM。第三，我们选择使用的数值方法为蒙特卡洛模拟。第四，我们构建将这些计算整合为一系列计算步骤的算法，这将作为我们在 C++ 中实现的蓝图。

算法在便当盒模板的方框 4 中显示。算法的 C++ 实现分别显示在代码片段 1、2 和 3 中。代码片段 1 (maineq1.cpp) 是定价算法本身，而代码片段 2 和 3 是辅助函数。该算法由六个步骤组成，从输入参数（STEP 1）一直延伸到权利金值的输出（STEP 6）。

该算法的一个重要特性是代码片段 2 (random.cpp) 中的函数。该函数实现了 Box-Muller 方法，以获取 GBM 所需的标准正态（高斯）分布的随机样本。代码片段 3 (random.h) 只是代码片段 2 (random.cpp) 的头文件。Box-Muller 方法从均匀分布中取两个独立样本，并将其转换为单个高斯分布样本；该值需要赋值给代码中的变量 epsilon。当然，也可以实现更高效的版本。Box-Muller 方法实际上将一对均匀变量转换为一对正态变量。最好也使用过程中生成的第二个正态样本，以提高计算效率。有关此更高效实现的详细信息，请参阅本书网站；有关进一步详细信息，请参阅原始论文（A Note on the Generation of Random Normal Deviates）。

作为输入参数的一部分，我们应定义 N 和 M。在此，N 表示 GBM 计算中使用的时间步数，而 M 表示使用的蒙特卡洛模拟次数。在我们的示例中，我们考虑对巴克莱股票（BARC.L）的欧式看涨期权进行定价，其当前价格为 £100，行权价 £100，无风险年利率 5%，年化波动率 10%，期限为一年。我们使用 N=500 和 M=10,000。在我的计算机上，期权权利金为 £6.81，执行时间为 1.34 秒。权利金值和执行时间会因计算机而异。

请注意，只需更改算法中的 STEP 4，此代码即可轻松修改以对其他支付函数进行定价。就 C++ 实现而言，可以使用类来定义支付函数，从而融入这一概念。此外，可以稍微修改 STEP 4 以包含对蒙特卡洛近似精度的估计。请参阅网站以获取包含这些功能的可下载实现。描述此示例的优秀教材是《金融期权定价导论：数学、随机过程与计算》。

### 代码 1 – EQ1 – 蒙特卡洛欧式看涨期权

以下是 EQ1_main.cpp 文件的代码片段：

```
// maineq1.cpp
// requires random.cpp
#include "random.h"
#include <iostream>
#include <cmath>
#include <algorithm>

using namespace std;
int main()
{
    cout << "\n *** START EQ1: Monte Carlo European Call *** \n";
    // STEP 1: INPUT PARAMETERS
    double T=1; // maturity
    double K=100; // strike
    double S0=100; // spot
    double sigma=0.10; // volatility
    double r=0.05; // interest rate
    int N=500; // number of steps
    int M=10000; // number of simulations
    double S[N+1];
    double sumpayoff=0;
    double premium=0;
    double dt = T / N;

    // STEP 2: MAIN SIMULATION LOOP
    for (int j=0; j < M; j++)
    {
        S[0]=S0; // initialize each path for simulation

        // STEP 3: TIME INTEGRATION LOOP
        for (int i=0; i < N; i++)
        {
            double epsilon = SampleBoxMuller(); // get Gaussian draw
            S[i+1] = S[i]*(1+r*dt+sigma*sqrt(dt)*epsilon);
        }
    }

    // STEP 4: COMPUTE PAYOFF
    sumpayoff += max(S[N]-K,0.0); // compute and add payoff

    // STEP 5: COMPUTE DISCOUNTED EXPECTED PAYOFF
```

```
premium = exp(-r*T)*(sumpayoff / M);

// STEP 6: OUTPUT RESULTS
cout <<"premium = " << premium << "\n";
cout << "\n *** END EQ1: Monte Carlo single asset *** \n";

return 0;
}
```

### 代码 2 - random.cpp 文件

以下是 random.cpp 文件的代码片段：

```
// random.cpp
// Computing Gaussian deviates using Box-Muller method

#include "Random.h"
#include <cstdlib>
#include <cmath>
using namespace std;

double SampleBoxMuller()
{
    double result;
    double x;
    double y;

    double xysquare;
    do
    {
        x = 2.0*rand()/static_cast<double>(RAND_MAX)-1;
        y = 2.0*rand()/static_cast<double>(RAND_MAX)-1;
        xysquare = x*x + y*y;
    }
    while
    (   xysquare >= 1.0 );
    result = x*sqrt( -2*log(xysquare)/xysquare );
    return result;
}
```
