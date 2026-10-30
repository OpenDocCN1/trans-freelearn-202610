# 索引

## A

accuracy
  基于误分类率  6
  灵敏度与特异度  7–8
area under ROC curves (AUC)
  双正态曲线  27–28
  双正态曲线，比较  42–46, 65–66
  AUC 的自助法置信区间  33–35
  ROC 曲线的自助法验证估计  107
  比较置信区间  48
  使用 FREQ 过程计算  23–24
  使用 LOGISTIC 过程计算  22–23
  使用 %ROC 宏计算  24–25
  ROC 曲线的交叉验证估计  103–104
  经验曲线  21–25
  经验曲线，比较  39–42, 64–65
  经验曲线，有序标记  54–55
  ROC 曲线的 Lehmann 家族  70
  有序标记  63–66
  带删失数据的 ROC 曲线  86–88
  ROC 曲线的分样本估计  100–102
asymptotic standard error (ASE)  12
AUC
  参见 area under ROC curves
AXIS statement  55

## B

binary predictor  5–14
  用于报告连续预测变量  25–26
  霜冻预报（示例）  5–6
  强调的原因  95
BINOMIAL keyword (FREQ procedure)  10, 12
binormal parameters  27
binormal ROC curves  26–30, 96
  下面积  27–28
  与经验曲线比较  29, 46–47
  比较下面积  42–46
  直接估计  32–33
latent  57–63
latent，下面积比较  65–66
normality  30
回归模型  49–51, 59–60, 69
标准摄取值（SUV）  26–27
向双正态的转换  30–32
binormal ROC curves, area under
  参见 area under ROC curves (AUC)
%BOOT1AUC macro  34, 56
bootstrap samples for confidence intervals  96
  用于曲线下面积（AUC）  33–35
  用于 AUC 差值  48
bootstrap-validated estimates of ROC curves  96, 106–107
Box-Cox power transformations  30–31, 43
%BVAL macro  107

## C

c-index  90–91, 93
cancer examples  38, 81–82
censored data, ROC curves with  81–93
  曲线下面积  86–88
  一致性概率  88–91
  单调函数与删失数据  86
%CINDEX macro  90
CLASS statement, UNIVARIATE procedure  16
"close to the ideal"  26
clustered data  75–79
comparing ROC curves  38
  用于有序预测变量  63–66
  ROC 曲线的 Lehmann 家族  79
  配对 vs. 非配对数据  38–39
competing models, ROC curves for  113–116
concordance probability  22
  另见 area under ROC curves (AUC)
  带删失数据的 ROC 曲线  88–91
  与 Cox 回归模型  91–93
conditioning  10
confidence intervals for sensitivity and specificity  10–12
confidence intervals from bootstrap samples  96
  用于曲线下面积（AUC）  33–35
  用于 AUC 差值  48
continuous predictor  15–35, 97–98
  二值化  15–18
continuous predictor (continued)
  ROC 曲线的 Lehmann 家族  67–80
  操作点  25–26
  最优阈值  25–26
  强调的原因  95
  使用二元预测变量报告  25–26
  ROC 曲线  18–20
  阈值  25–26
CONTRAST statement, NLMIXED procedure  44–46
CONTRAST variable, %ROC macro  39–40
covariate adjustment for ROC curves  48–49
  Lehmann 家族  73–75
COVSANDWITCH option, PHREG procedure  76–77
Cox regression models  69, 91–93
%CPE macro  93
credit rating example  54
credit scoring  2
cross-validation estimates of ROC curves  96, 102–106
  曲线下面积  103–104
  k 折交叉验证  96, 104–106
  LOGISTIC 过程  102–104
  重代入估计 vs.  104
D
data clustering  75–79
decision trees  113
dichotomizing continuous predictors  15–18
discordant pairs
  参见 concordance probability
double dipping  96
E
empirical ROC curves  20–26, 63–66
  与双正态曲线比较  29, 46–47
  比较下面积  39–42
  用于有序预测变量  54–57, 64–65
empirical ROC curves, area under
  参见 area under ROC curves (AUC)
ESTIMATE statement, NLMIXED procedure  28, 44–46, 62
estimating equations for clustered data  75–79
Euclidean method  25
exported data, GPLOT procedure with  116–118
F
false negatives (FN)  6, 7–8
  使用 FREQ 过程计算  9
false positives (FP)  6, 7–8
  使用 FREQ 过程计算  9
"far from random"  26
FREQ procedure  8–14
  BINOMIAL keyword  10, 12
  计算假阴性和假阳性  9
  计算误分类率  9
  计算阴性和阳性预测值  9
  计算灵敏度  9–12
  计算特异度  9–10, 13
  计算 Somer's D 和 AUC  23–24
  ORDER= 选项  14
  TABLE statement  23
FRONTREF option, HISTOGRAM statement (UNIVARIATE)  17
frost forecast (example)  5–6
G
generalized estimating equations  76
GPLOT procedure  116–118
H
hazard function  68
heteroscedastic models  51
HISTOGRAM statement, UNIVARIATE procedure  16
  FRONTREF option  17
  HREF= option  17
home equity loan example  110
homoscedastic models  51
HREF= option, HISTOGRAM statement  17
I
INMODEL= option, LOGISTIC procedure  100
intercept  27
invariance to monotone transformations  21, 22
K
k-fold cross-validation  96, 104–106
Kaplan-Meier estimation method  83
L
latent binormal ROC curves 57–63
  比较下面积 65–66
leave-one-out validation 96, 102–104
leaves (SAS Enterprise Miner) 114
Lehmann family of ROC curves 67–80
  协变量调整 73–75
  曲线下面积 70
  聚类数据 75–79
  比较曲线 79
LIFETEST procedure 83–85
  TIME statement 83
liver surgery example 96–97
location-scale families 59
LOGISTIC procedure 8, 19
  计算 AUC 22–23
  ROC 曲线的交叉验证估计 102–104
  INMODEL= option 100
  OUTMODEL= option 100
  ROC 曲线的重代入估计 96
  SCORE statement 100, 101
  ROC 曲线的分样本估计 100–102
lung cancer example 81–82
M
magnetic resonance example 70–73
MEASURES option, TABLE statement (FREQ) 23
METHOD= option, SURVEYSELECT procedure 34
misclassification rate (MR) 6–7
  使用 FREQ 过程计算 9
  在 ROC 曲线中 19
MIXED procedure 44
  双正态 ROC 曲线的回归模型 49–50
MODEL statement
  OUTROC= option 19, 71, 101, 104
  RL option 72
  SLE option 97
  SLS option 97
monotone functions 21
  删失数据和 86
monotone transformations, invariance to 21, 22
MR
  参见 misclassification rate
multiple observations from individual unit 75–79
multivariable prediction models 95–107
  ROC 曲线的自助法验证估计 96, 106–107
  ROC 曲线的交叉验证估计 96, 102–106
  ROC 曲线的重代入估计 96, 97–99, 102, 104
  ROC 曲线的分样本估计 96, 99–102
N
negative predictive value (NPV) 8
  使用 FREQ 过程计算 9
neural networks 114–115
NLMIXED procedure 27–28
  CONTRAST statement 44–46
  ESTIMATE statement 28, 44–46, 62
  估计双正态 ROC 曲线 32–33
  有序-概率回归模型 60–62
  RANDOM statement 44
  双正态 ROC 曲线的回归模型 49–50
nodes (SAS Enterprise Miner) 111, 114
NOPRINT option, TABLE statement (FREQ) 23
normality, in binormal models 30
NPV (negative predictive value) 8
  使用 FREQ 过程计算 9
O
observations, multiple from individual unit 75–79
observed operating points 19
OFFSET= option, AXIS statement 55
operating point
  continuous predictor 25–26
  observed 19
OPTIMAL option, %PLOTROC macro 26
optimal threshold, continuous predictor 25–26
optimism-adjusted estimates 106
ORDER= option, FREQ procedure 14
ordinal predictors 53–66
  比较 ROC 曲线 63–66
  有序预测变量的经验 ROC 曲线 54–57, 64–65
  潜在双正态 ROC 曲线 57–63
  有序-概率回归模型 60–62
OUT= option, SURVEYSELECT procedure 34
OUTHITS= option, SURVEYSELECT procedure 34–35
OUTMODEL= option, LOGISTIC procedure 100
OUTPUT statement, PREDPROBS=X option 103, 104
OUTROC= option
  MODEL statement 19, 71, 101, 104
  SCORE statement (LOGISTIC) 101
P
paired data 38–39
  比较双正态 ROC 曲线 42–46
  比较经验 ROC 曲线 39–41
PARAMETER= option, BOXCOX transformation 31
partitioning, recursive 113
percent correct statistic 6
PET scanning (example) 16–17
PHREG procedure 69–70, 72–73, 74
  聚类数据 75–79
  COVSANDWITCH option 76–77
%PLOTROC macro 41, 100–101, 105, 116
  OPTIMAL option 26
positive predictive value (PPV) 8
  使用 FREQ 过程计算 9
prediction
  business of 1
  in medical diagnostics 2
prediction models, multivariable
  参见 multivariable prediction models
predictors
  另见 binary predictor
  另见 continuous predictor
  二值化连续 15–18
  有序 53–66
PREDPROBS=X option, OUTPUT statement 103, 104
prevalence 7
product limit estimation method 83
proportional hazards regression 69, 79
  与一致性概率 91–93
prostate cancer prognosis (example) 38
R
RANDOM statement, NLMIXED procedure 44
randomly breaking the ties 22
receiver operating characteristic curves
  参见 ROC curves
recursive partitioning 113
REG procedure 32–33
regression
  Cox regression models 69, 91–93
  双正态 ROC 曲线的模型 49–51, 69
  双正态 ROC 曲线的有序模型 59–60
  有序-概率模型 60–62
  比例风险回归 69, 79, 91–93
resubstitution estimates of ROC curves 96, 97–99, 102
  与交叉验证估计比较 104
  LOGISTIC 过程 96
  与分样本估计比较 102
RL option, MODEL statement 72
ROC curves 1–3
  另见 binormal ROC curves
  另见 empirical ROC curves
  ROC 曲线的自助法验证估计 96, 106–107
  ROC 曲线的协变量调整 48–49, 73–75
  ROC 曲线的交叉验证估计 96, 102–106
  用于竞争模型 113–116
  用于单个连续预测变量 18–20
  用于单个模型 111–113
  在 SAS Enterprise Miner 中 109–118
  ROC 曲线的 Lehmann 家族 67–80
  ROC 曲线中的误分类率 19
  多变量预测模型 95–107
  命名法 2
  ROC 曲线的重代入估计 96, 97–99, 102, 104
  半参数 68
  平滑 26
  ROC 曲线的分样本估计 96, 99–102
  带删失数据 81–93




ROC 曲线，曲线下面积
参见 ROC 曲线下面积（AUC）
ROC 曲线，比较 38
对于有序预测因子 63–66
Lehmann 家族的 ROC 曲线 79
配对 vs. 未配对数据 38–39
%ROC 宏 24–25, 103
比较经验曲线 39–42, 64–65
CONTRAST 变量 39–40
具有有序标记的经验曲线 56–57
ROC 点 54–55
另见 有序预测因子
%ROCPLOT 宏 20
S
SAS Enterprise Miner 109–118
使用导出数据的 GPLOT 过程 116–118
叶节点 114
节点 111, 114
竞争模型的 ROC 曲线 113–116
单一模型的 ROC 曲线 111–113
终端节点 114
SCORE 语句，LOGISTIC 过程 100
OUTROC= 选项 101
自预测 96
半参数 ROC 曲线 68
灵敏度 7–8
使用 FREQ 过程计算 9–12
置信区间 10–12
相对于特异性作图 18–19
带删失数据的 ROC 曲线 82–83
单一二元预测因子 5–14
用于报告连续预测因子 25–26
霜冻预报（示例）5–6
强调的原因 95
单一连续预测因子 15–35, 97–98
二值化 15–18
Lehmann 家族的 ROC 曲线 67–80
强调的原因 95
使用二元预测因子报告 25–26
ROC 曲线 18–20
单一模型，ROC 曲线 111–113
SLE 选项，MODEL 语句 97
斜率 27
SLS 选项，MODEL 语句 97
平滑 ROC 曲线 26
Somer's D 统计量 23–24
特异性 7–8
使用 FREQ 过程计算 9–10, 13
置信区间 10–12
相对于灵敏度作图 18–19
带删失数据的 ROC 曲线 82–83
ROC 曲线的分割样本估计 96, 99–102
曲线下面积 100–102
用于此的 LOGISTIC 过程 100–102
重代入估计 vs. 102
分层分割 99
逐步选择 97–98
分层分割 99
SURVEYSELECT 过程 34–35
METHOD= 选项 34
OUT= 选项 34
OUTHITS= 选项 34–35
生存模型 82–83
SUV（标准化摄取值）16–17
双变量曲线 26–27
二值化 17
直方图 16, 17
T
TABLE 语句，FREQ 过程 23
MEASURES 选项 23
NOPRINT 选项 23
%TDROC 宏 86–88
终端节点（SAS Enterprise Miner）114
阈值，连续预测因子 25–26
TIME 语句，LIFETEST 过程 83
TNR（真阴性率）
参见 特异性
TPR（真阳性率）
参见 灵敏度
TRANSREG 过程 30–32, 43
梯形法则 21–22
真阴性率（TNR）
参见 特异性
真阴性（TN）6, 9
真阳性率（TPR）
参见 灵敏度
真阳性（TP）6, 9
两样本 t 检验 38

## U
U 统计量 39

用于 SUV 直方图的 UNIVARIATE 过程
16

未配对数据 38–39
比较双变量 ROC 曲线 46
比较经验 ROC 曲线 41–42

## V
验证 96
自助法验证的 ROC 曲线估计 96, 106–107
交叉验证估计的 ROC 曲线 96, 102–106
留一法验证 96, 102–104
重代入估计的 ROC 曲线 96, 97–99, 102, 104
分割样本估计的 ROC 曲线 96, 99–102

## W
Wald 检验 12, 79
天气预报示例 1, 5–6

## X
%XVAL 宏 104–105

## 来自 SAS Press 的书籍

使用 SAS® 的高级对数线性模型
作者 Daniel Zelterman

使用 SAS® 进行临床试验分析：实用指南
作者 Alex Dmitrienko, Geert Molenberghs, Walter Offen, 和 Christy Chuang-Stein

使用 SAS® 分析接收者操作特征曲线
作者 Mithat Gönen

Annotate: Simply the Basics
作者 Art Carpenter

使用 SAS® 软件的第二版 应用多元统计
作者 Ravindra Khattree 和 Dayanand N. Naik

第五版 应用统计学与 SAS® 编程语言
作者 Ronald P. Cody 和 Jeffrey K. Smith

An Array of Challenges — Test Your SAS® Skills
作者 Robert Virgile

使用 SAS/IntrNet® 构建 Web 应用程序：应用程序调度器的指南
作者 Don Henderson

第二版 Carpenter 的 SAS® 宏语言完全指南
作者 Art Carpenter

Carpenter 的 SAS® REPORT 过程完全指南
作者 Art Carpenter

统计学漫画指南
作者 Larry Gonick 和 Woollcott Smith

第二版 使用 SAS® 系统进行分类数据分析
作者 Maura E. Stokes, Charles S. Davis, 和 Gary G. Koch

使用 SAS® 软件进行数据清理技术
作者 Ron Cody

第二版 使用 SAS® 示例进行临床研究的常用统计方法
作者 Glenn A. Walker

SAS® 索引的完整指南
作者 Michael A. Raithel

使用 SAS® Enterprise Miner™ 进行 CRM 分段和聚类
作者 Randall S. Collica

使用 SAS® Learning Edition 2.0 进行数据管理和报告变得简单
作者 Sunil K. Gupta

使用 SAS® 进行分析的数据准备
作者 Gerhard Svolba

调试 SAS® 程序：工具和技术手册
作者 Michele M. Burlew

使用 SAS® Enterprise Miner™ 进行商业智能和数据挖掘的决策树
作者 Barry de Ville

效率：提高 SAS® 应用程序的性能
作者 Robert Virgile

SAS® 日期和时间的必备指南
作者 Derek P. Morgan

SAS® 用户的 SAS/GRAPH® 软件如何操作手册
作者 Thomas Miron

SAS® 提示和技巧来自全球各地，第二版
作者 Phil Mason

Instant ODS：输出传送系统的样式模板
作者 Bernadette Johnson

使用 SAS® 软件通过 Meta-分析综述整合结果
作者 Morgan C. Wang 和 Brad J. Bushman

使用 SAS® Enterprise Miner™ 快速入门数据挖掘
作者 Patricia B. Cerrito

通过示例学习 SAS®：程序员指南
作者 Ron Cody

第二版 在计算机实验室学习 SAS®
作者 Rebecca J. Elliott

SAS® 小书：入门
作者 Lora D. Delwiche
和 Susan J. Slaughter

SAS® 小书：入门，第二版
作者 Lora D. Delwiche
和 Susan J. Slaughter
（更新至包含 SAS 7 功能）

SAS® 小书：入门，第三版
作者 Lora D. Delwiche
和 Susan J. Slaughter
（更新至包含 SAS 9.1 功能）

Enterprise Guide® 3.0 的 SAS® 小书
作者 Susan J. Slaughter
和 Lora D. Delwiche

Enterprise Guide® 4.1 的 SAS® 小书
作者 Susan J. Slaughter
和 Lora D. Delwiche

使用 SAS® 系统的逻辑回归：
理论与应用
作者 Paul D. Allison

SAS® 的纵向数据：程序员指南
作者 Ron Cody

使用 SAS® 轻松制作地图
作者 Mike Zdeb

使用 JMP® 进行测量、分析和控制：制造的质量技术
作者 Jack E. Reece

离散数据的模型
作者 Daniel Zelterman

使用 SAS® 进行多重比较和多重检验 文本和工作簿套装
（本书套装中的书籍也可单独出售）
作者 Peter H. Westfall, Randall D. Tobias,
Dror Rom, Russell D. Wolfinger,
和 Yosef Hochberg

使用宏简化多图显示
作者 Perry Watts

使用 SAS® 软件进行多元数据降维和判别
作者 Ravindra Khattree
和 Dayanand N. Naik

输出传送系统：基础知识
作者 Lauren E. Haworth

SAS® 用户的无痛 Windows 手册，第三版
作者 Jodie Gilmore
（更新至包含 SAS 8 和 SAS 9.1 功能）

使用 SAS® 进行制药统计：实用指南
编辑 Alex Dmitrienko, Christy Chuang-Stein,
和 Ralph D'Agostino

PROC FORMAT 的威力
作者 Jonas V. Bilenas

使用 SAS® Enterprise Miner™ 进行预测建模：商业应用的实用解决方案
作者 Kattamuri S. Sarma, Ph.D.

使用 SAS® 进行 PROC SQL：基础知识之外
作者 Kirk Paul Lafler

PROC TABULATE 示例
作者 Lauren E. Haworth

第五版 专业 SAS® 程序员口袋参考
作者 Rick Aster

第二版 专业 SAS® 编程快捷方式
作者 Rick Aster

使用 SAS/GRAPH® 软件快速获得结果
作者 Arthur L. Carpenter
和 Charles E. Shipp

使用输出传送系统快速获得结果
作者 Sunil K. Gupta

使用 SAS® 读取外部数据文件：示例手册
作者 Michele M. Burlew

使用 SAS® 软件进行回归和方差分析：综合方法
作者 Keith E. Muller
和 Bethel A. Fetterman

SAS® For Dummies®
作者 Stephen McDaniel
和 Chris Hemedinger

第二版 使用 SAS® 进行时间序列预测
作者 John C. Brocklebank
和 David A. Dickey

第四版 SAS® 用于线性模型
作者 Ramon C. Littell, Walter W. Stroup,
和 Rudolf J. Freund

第二版 SAS® 用于混合模型
作者 Ramon C. Littell, George A. Milliken, Walter W. Stroup,
Russell D. Wolfinger, 和 Oliver Schabenberger

SAS® 用于蒙特卡洛研究：定量研究者的指南
作者 Xitao Fan, Ákos Felsővályi, Stephen A. Sivo,
和 Sean C. Keenan

通过示例的 SAS® 函数
作者 Ron Cody

SAS® 用于 Java 的图形：使用 SAS® AppDev Studio™ 和输出传送系统的示例
作者 Wendy Bohnenkamp
和 Jackie Iverson

第二版 SAS Guide to Report Writing
作者 Michele M. Burlew



SAS® 宏编程轻松入门，第二版
作者 Michele M. Burlew

SAS® 编程实例
作者 Ron Cody
和 Ray Pass

SAS® 研究人员与社会科学家的编程，第二版
作者 Paul E. Spector

SAS® 制药行业编程
作者 Jack Shostak

SAS® 医学研究生存分析技术，第二版
作者 Alan B. Cantor

SAS® 系统初级统计分析，第二版
作者 Sandra D. Schlotzhauer
和 Ramon C. Littell

SAS® 系统回归分析，第三版
作者 Rudolf J. Freund
和 Ramon C. Littell

SAS® 系统统计图形，第一版
作者 Michael Friendly

SAS® 工作簿与解答集
（本套书中的书籍也可单独出售）
作者 Ron Cody

使用 SAS® 节省时间和金钱
作者 Philip R. Holland

为 SAS® 用户选择社会科学数据的统计技术：指南
作者 Frank M. Andrews, Laura Klem, Patrick M. O'Malley, Willard L. Rodgers, Kathleen B. Welch, 和 Terrence N. Davidson

使用 SAS® 系统进行统计质量控制
作者 Dennis W. King

使用 SAS® Enterprise Guide® 进行统计
作者 James B. Davis

逐步使用 SAS® 系统进行因子分析和结构方程建模的方法
作者 Larry Hatcher

逐步使用 SAS® 进行单变量和多变量统计的方法，第二版
作者 Norm O'Rourke, Larry Hatcher, 和 Edward J. Stepanski

使用 SAS® 逐步学习基础统计：学生指南与练习
（本套书中的书籍也可单独出售）
作者 Larry Hatcher

使用 SAS® 进行生存分析：实用指南
作者 Paul D. Allison

在 OS/390 和 z/OS 环境中优化 SAS® 应用，第二版
作者 Michael A. Raithel

单变量和多变量广义线性模型：使用 SAS® 软件的理论与应用
作者 Neil H. Timm 和 Tammy A. Mieczkowski

使用 SAS® 进行金融研究
作者 Ekkehart Boehmer, John Paul Broussard, 和 Juha-Pekka Kallunki

使用 SAS® 窗口环境：快速教程
作者 Larry Hatcher

可视化分类数据
作者 Michael Friendly

使用示例进行 SAS® Web 开发，第二版
作者 Frederick E. Pratter

使用 SAS® 系统进行调查研究的指南
作者 Archer Gravely

## JMP® 书籍

使用 JMP® 进行初级统计
作者 Sandra D. Schlotzhauer

JMP® 基础单变量和多变量统计：逐步指南
作者 Ann Lehman, Norm O'Rourke, Larry Hatcher, 和 Edward J. Stepanski

JMP® Start Statistics，第三版
作者 John Sall, Ann Lehman, 和 Lee Creighton

使用 JMP® 进行回归
作者 Rudolf J. Freund, Ramon C. Littell, 和 Lee Creighton
