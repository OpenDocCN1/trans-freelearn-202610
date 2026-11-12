## C++ 期权定价数值库

在 C++ 中实现金融衍生品可能是一项复杂的任务。如本书所示，这不仅需要掌握以 C++ 代码形式实现所需的数学模型和数值方法，还需要使用可靠的支持性数学和金融库。例如，当您需要从标准正态分布中获取随机样本或需要对矩阵求逆时。在这些情况下，与其从零开始实现这些算法，不如利用为此目的存在的数值库。这些库包含多年来一直被使用、并已被众多用户验证的算法。使用这些库将显著加速我们对高级定价模型的实现。即将在接下来的章节中介绍这些库的一些示例。

### 数值方法（Numerical Recipes）

许可证：商业。

网站：http://www.nr.com。

广泛使用且可靠的 C++ 数值例程集合收录在书籍《数值方法：科学计算艺术，第 3 版》（Numerical Recipes: The Art of Scientific Computing, 3rd Edition）中。这些例程被全球顶尖大学和研究机构视为“金标准”。有一个出色的关联网站可在 http://www.nr.com/ 找到。该书包含例程的理论背景描述，并提供 C++ 代码访问权限。书中包含超过 400 个 C++ 数值例程，涵盖线性代数方程求解、矩阵代数、插值与外推、积分和随机数等主题。

### 金融数值方法

许可证：免费/GNU。

网站：http://finance.bi.no/~bernt/gcc_prog/。

该网站包含由 Bernt Arne Odegaard 开发的大量非常有用的 C++ 数值和金融程序。它们遵循 ANSI C++ 标准，并附带一份名为 Circa 的大型手册（250 页），其中包含使用的公式和相关参考资料。该库可在 http://finance.bi.no/~bernt/gcc_prog/ 找到。

### QuantLib 项目

许可证：免费/GNU。

网站：http://quantlib.org/。

QuantLib 项目是一个大型项目，为定量金融提供软件。该软件已在金融领域用于建模、交易和风险管理。该软件使用 C++ 编写，随后已导出至多种语言，如 C#、Objective Caml、Java、Perl、Python、GNU R、Ruby 和 Scheme。QuantLib 拥有许多有用的工具，包括收益率曲线模型、求解器、偏微分方程、蒙特卡洛（低差异）、异国期权、VAR 等。

### Boost 库

许可证：免费/GNU。

网站：www.boost.org。

Boost 项目提供经过同行评审的可移植 C++ 源代码库，可在 GNU GPL 下免费使用。这些库的创建旨在使其在广泛的应用领域中实用且可用。十个 Boost 库被收录在 C++ 标准委员会的库技术报告（TR1）和新的 C++11 标准中。示例包括 Accumulators、Array、Chrono、Filesystem、Geometry、Math、Math/Statistical Distributions 和 MPI。





### GSL 库

许可证：免费/GNU。

网站：www.gnu.org/s/gsl/。

GNU 科学库（GSL）是面向 C 和 C++ 的数值库。该库提供大量数学数值例程，包括随机数生成器、特殊函数和最小二乘拟合。总计超过 1000 个函数。该库涵盖的学科领域示例包括复数、多项式根、特殊函数、向量与矩阵、排列、线性代数、特征系统、快速傅里叶变换、数值积分、随机数、准随机序列、统计学、直方图和蒙特卡洛积分。
