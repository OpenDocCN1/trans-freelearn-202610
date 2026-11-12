## 使用 C++ 的信用衍生品

在本最后一章中，我们将重点介绍 C++ 在信用衍生品定价中的应用。我们将考虑两个示例：使用 Merton 模型对可违约公司的股权以及公司的违约概率进行定价（基本示例）和信用违约互换（CDS）的定价（高级示例）。第一个示例基于信用风险的结构方法，而第二个示例基于强度方法。我们为这两个示例提供了完整可用的 C++ 实现。更简单的 C 实现（不包含 OO 特性）可在随书网站上找到。

### 基本示例 – 破产 (CR1)

在本示例中，我们将使用（Merton 1974）模型研究公司的违约（破产）。有关更多信息，请参阅 "On the Pricing of Corporate Debt: The Risk Structure of Interest Rates"。在该模型中，公司的动态使用几何布朗运动（GBM）来描述。假设公司的资本结构非常简单：公司的资产（V）完全由股权（E）和债务（D）组成。对于给定的期限 T，如果公司在期末的资产 V(T) 少于公司在那个时间点需要支付的债务价值（D），则发生违约。

在此背景下，我们可以使用蒙特卡洛模拟来研究公司的违约概率。通过生成公司可能演化的若干条可能轨迹，并统计公司满足违约条件的次数，我们可以估计公司破产的可能性。

我们的目标是计算 $V(T)$ 少于债务价值（$D$）的次数，并将其用作 **违约概率 (PD)** 的估计。除了使用相同的蒙特卡洛计算外，我们还可以估计公司在 *time*=0 时的股权价值。

我们现在按顺时针方向填写便当盒的内容，从左上角开始。首先，我们填写破产分析的所有数据，特别是违约条件，在我们的示例中如下：

$V_T < D$

其次，我们应为基础资产选择数学模型，在 Merton 模型中为 GBM。第三，我们选择要使用的数值方法，在此我们选择蒙特卡洛模拟。第四，我们构建将这些计算组合为一系列计算步骤的算法，这将作为我们在 C++ 中实现该算法的蓝图。

该算法显示在破产的便当盒模板的方框 4 中。该算法在 C++ 中的实现显示在代码片段 1 中。该算法由七个步骤组成，将引导我们从输入参数（**STEP 1**）到保费价值的输出（**STEP 6**）。

蒙特卡洛模拟需要随机数生成器才能运行，因此，`random.cpp` 文件（在 *第 4 章，使用 C++ 的股权衍生品* 中学习过）被重复使用。

我们考虑一家公司的示例，其资本结构由 t=0 时的公司总资产 V(0)=100 百万欧元，以及由面值为 D=70 百万欧元的单一零息债券组成的债务构成。假设公司资产的波动率为 20%。期限为四年。无风险利率为每年 5%。

运行如下图所示的 C++ 代码片段，使用 500 个步骤和 10,000 次模拟，我们估计四年期内的违约概率为 88.63% 且股权价值为 $E(0)=43.95$ 百万欧元：

### 1 破产

公司的资产（V）是公司股权（E）和公司债务（D）之和。如果资产价值 V(T) 少于到期时需要支付的债务 D，则公司违约。Merton 表明，公司的股权是一种带有以下收益的欧洲看涨期权：

\( H(V_T) = max(V_T - D, 0) \)

### 2

根据 Merton 模型，**公司**可以使用几何布朗运动（GBM）来描述：

\( dV = rVdt + \sigma VdW \)

### 3

使用 **蒙特卡洛模拟** 作为计算公司股权价值的方法，作为折现预期收益。

\( \pi = exp(-rT) \times E^{\mathbb{Q}}[H(V_T)] \)

- **4**
- STEP 1: 输入参数
- STEP 2: 主模拟循环
- for i=1:M
- STEP 3: 时间积分循环
- for i=1:N
- *在每个时间步长计算公司价值 V 的 GBM 公式*
- end
- STEP 4: 计算收益
- STEP 5: 计算违约次数
- end
- STEP 6: 计算折现预期收益
- STEP 7: 输出股权价值和违约概率

公司破产的便当盒模板 (CR1)

接下来的代码片段实现了便当盒模板中的算法。

### 代码 21 – CR1_main.cpp (使用 Merton 模型的破产)

以下是 CR1_main.cpp 文件的代码片段：

```cpp
// CR2_main.cpp
// It requires CR2_source.cpp
#include "CR2.h"
#include <iostream>
using namespace std;
int main()
```

```cpp
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

### 代码 22 – CR1_source.cpp (使用 Merton 模型的破产)

以下是 CR1_source.cpp 文件的代码片段：

```cpp
// CR2_source.cpp

#include "CR2.H"
#include <vector>
```

```cpp
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

有关代码片段 23 CR1.h，请参阅代码包中的代码。

要计算基本示例 (CR1)，您需要编译并运行代码片段 21、22、23、4 和 5（这些片段包含头文件和随机函数）；之后，您应该获得以下截图：

![](img/29d9d2cdc85db7e20e91367a93390be2_104_0.png)

公司破产 (CR1) 截图，显示结果

### 高级示例 – CDS (CR2)

在第二个示例中，我们考虑 CDS 定价。该方法的详细信息显示在以下 CDS 的便当盒模板中：

信用违约互换 (CDS)，交易对手为 A 和 B。A 向 B 支付定期保费，以换取信用保护（基础资产 C 的违约，名义本金为 N）。如果 C 违约，B 向 A 支付一次性赔偿，价值为 N(1-R)。CDS 的价格是 CDS 利差或保费。

在强度框架内，基础资产 C 的生存概率 P(t) 由以下公式给出：P(t) = exp(-λt)

CDS 定价涉及确定性计算。保费可以通过将保费腿和违约腿的现值相等来计算。

- STEP 1: 输入参数
- STEP 2: 遍历所有付款的循环
- for i=1:M
- STEP 3: 计算保费腿 (PL) 付款
- STEP 4: 计算违约腿 (DL) 付款
- end
- STEP 5: 计算 CDS 利差
- STEP 6: 输出结果

CDS (CR2) 的便当盒模板

CDS 是两个交易对手 A 和 B 之间的金融合同，其中一方向另一方付款以购买针对基础资产 C 可能违约的信用保护。

从结构上看，CDS 与普通利率互换（IRS）相似，因为它由各方之间的现金流交换组成。在典型的五年期 CDS 中，交易对手 A 在约定名义本金上定期向 B 支付一系列保费。只要基础资产 C “存活”（即不违约），这些付款就会继续进行。





对手方 B 在基础资产 C 违约时向 A 支付单次或有付款。支付金额等于名义本金减去回收率。用数学表达式表示如下：

$$N(1 - R)$$

与 IRS 类似，合约的“价格”是通过计算每条腿的现值得到的（预期溢价付款之和称为**溢价腿 (PL)**，预期违约付款之和称为**违约腿 (DL)**）。用数学表达式，PL 和 DL 表示如下：

$$PL = \sum_{i=1}^{N} \pi \times N \times P(T_i) \times \Delta t \times DF_i$$

$$DL = \sum_{i=1}^{N} N \times (1 - R) \times \left[ P(T_{i-1}) - P(T_i) \right] \times DF_i$$

在上述方程中，P(T) 是时刻 t 的生存概率，N 是名义本金，R 是回收率，DF(t) 是时刻 t 的贴现因子。为了公平定价，这些腿必须相等，在此基础上，我们可以确定应支付溢价的公允价值（也称为 CDS 利差）。该利差由希腊字母 $\pi$ 表示，被视为 CDS 合约的价格。用数学表达式，它可表示如下：

$$\pi = \frac{ \sum_{i=1}^{N} \pi \times N \times P(T_i) \times \Delta t \times DF_i }{ \sum_{i=1}^{N} N \times (1 - R) \times \left[ P(T_{i-1}) - P(T_i) \right] \times DF_i }$$

我们在 CDS 的 Bento Box 模板中呈现的定价算法试图从上述方程计算溢价。

可以看出，此计算是确定性的，因此不需要蒙特卡洛模拟。此处阐述的信贷模型是“基于强度的模型”的示例，其基于著作《受信用风险影响的金融证券衍生品定价》（Pricing Derivatives on Financial Securities Subject to Credit Risk）。

代码片段 2 说明了此定价在 C++ 中的实现。

我们将考虑如下示例：合约期限为一年，按季度付款（即每年四次付款），名义本金 = 1 亿美元，无风险利率 = 5% 年利率，基础资产风险率 = 1% 年利率，回收率 = 50%。对于这些输入，CDS 利差为 50.00626 个基点。

在下图中，我们看到 Bento Box 框架应用于我们的 CDS 问题：

![](img/29d9d2cdc85db7e20e91367a93390be2_107_0.png)

CDS 的 Bento Box 模板（CR2）

即将展示的代码片段实现了 Bento Box 模板中的算法。

### 代码 24 - CR2_main.cpp (CDS)

以下是 CR2_main.cpp 文件的代码片段：

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

### 代码 25 - CR2_source.cpp (CDS)

以下是 CR2_source.cpp 文件的代码片段：

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

> 有关代码片段 26 CR2.h，请参阅代码包中的代码。

要计算高级示例 (CR2)，需要编译并运行代码片段 24、25 和 26；随后，您应获得以下截图：

![](img/29d9d2cdc85db7e20e91367a93390be2_110_0.png)

CDS (CR2) 带结果的截图

### 摘要

在本章中，我们解决了信贷衍生品的两个定价问题。我们看到过一个基本示例（使用结构模型）和一个更高级的示例（使用强度模型）。虽然存在许多可能的变体和更复杂的合约，但这两个是主要家族，能让你对如何在这个迷人的资产类别中继续前进有所了解。至此，我们关于在 C++ 中实现不同类型金融衍生品的示例调查结束。
