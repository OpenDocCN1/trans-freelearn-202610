
上部显示方法，下部（再次分为两个部分）显示所选方法的子调用和父调用。要选择一个方法，双击它，它将在下部显示子调用和父调用。

用于限制显示的调用类型，可以使用以下按钮：

| Button | Type of calls |
|---|---|
| Profile line | Opens the profile lines form to show all lines of code that are executed. |
| Setup | This has four items to limit the type of calls displayed
None: No filtering, view all fields.
Code hot spots: View only the Duration, Share – duration, Method calls, and Share – method calls fields.
Database access: View only the Method calls, Share – method calls, and Calculated SQL time fields.
Remote connection: View only the Method calls, Share – method calls, and Calculated AOS time fields. |

## Totals Forms

**Totals** 表单用于显示汇总数据。各种 **Totals** 表单可从 **Tools | Development tools | Code profiler | Profiler runs | Totals** 访问。

Line total form: Displays aggregated data about total amount of time spent on executing each line of code for a code profiler run.

Method total form: Displays aggregated data about total amount of time spent on executing each method for a code profiler run.

Object total form: Displays aggregated data about total amount of time spent on executing each object for a code profiler run.

以下是三个表单上可用按钮的摘要：

| Button name and Function | Line total form | Method total form | Object total form |
|---|---|---|---|
| Profile lines: Opens Profile lines form | Yes | Yes | No |
| Line total: Opens Line total form | No | Yes | Yes |
| Method total: Opens Method total form | No | No | Yes |

## Profile Summary Form

此表单用于计算代码分析器运行中每行代码、每个方法和每个对象的总执行时间。当在创建运行时在代码分析器表单上清除“计算行总计”复选框时使用此表单；否则总计计算会自动完成。

> 代码分析器工具将很快被 "Trace Parser" 跟踪分析器取代。

## Dynamics AX Benchmark Toolkit

用于性能测试的 'Dynamics AX Benchmark Toolkit' 是一个随 **Microsoft Dynamics AX Resource Kit** 提供的外部工具。与 Axapta 3.0 中的基准测试工具相比，它具有以下明显优势。

- 更广泛的功能覆盖。
- 与 Dynamics AX 客户端相比，轻量级的业务连接器。
- 'Dynamics AX Benchmark Toolkit' 是一个用 C# 编写的托管程序集。它有两个主要库，即 The Programming Model Library 和 The Microsoft Dynamics AX Wrapper Library。第一个库用于通过业务连接器提供访问 Dynamics AX 的网关。后一个库 (Microsoft Dynamics AX Wrapper) 用于生成类型安全的托管类以与 Microsoft Dynamics AX 接口。使用此工具的前提是了解 .NET 和 X++，因此本书中未讨论此工具。

## Summary

我们讨论了一些 Dynamics AX 内置工具，它们可能在使测试过程更高效、更有效方面发挥重要作用。Dynamics AX 还有一个单元测试框架，这超出了本书的范围，因为单元测试通常由开发团队本身执行。

## Index

A
Access Control List. See ACL
访问控制列表。参见 ACL
ACL
about 86
关于 86
tips 86
提示 86
advantages, ERP system
优势，ERP 系统
management control 9
管理控制 9
operational control 9
运营控制 9
strategic planning 10
战略规划 10
AIF. See Application Integration framework
AIF。参见 Application Integration framework
AOT
element 34
元素 34
application design standards, Dynamics AX
应用程序设计标准，Dynamics AX
Auto property settings 53
自动属性设置 53
code placement 39
代码放置 39
field groups using, in table 52
在表中使用字段组 52
performance optimization 43
性能优化 43
Application Integration Framework 15
Application Integration Framework 15
Application Object Tree. See AOT
应用程序对象树。参见 AOT
authorization testing
授权测试
about 102
关于 102
specifications 102, 103
规范 102, 103
SysBPCheckIgnore Macro 137
SysBPCheckIgnore 宏 137
bug
错误
priority 124
优先级 124
security bugs 126
安全错误 126
B
back up and restore testing
备份和恢复测试
about 107
关于 107
specifications 107
规范 107
best practice check tool, Dynamics AX
最佳实践检查工具，Dynamics AX
applicable to all AOT elements 134
适用于所有 AOT 元素 134
applicable to specific AOT elements
适用于特定 AOT 元素
135-137
BP Deviation documented, comment 137
BP 偏差已记录，注释 137
category information 134
类别信息 134
invoking manually 133
手动调用 133
C
caching
缓存
about 44
关于 44
CacheLookup property 45, 46
CacheLookup 属性 45, 46
EntireTable, CacheLookup property 46
EntireTable，CacheLookup 属性 46
Found, CacheLookup property 45
Found，CacheLookup 属性 45
FoundAndEmpty, CacheLookup property 45
FoundAndEmpty，CacheLookup 属性 45
mechanism 45, 46
机制 45, 46
None, CacheLookup property 45
None，CacheLookup 属性 45
NotITTS, CacheLookup property 45
NotITTS，CacheLookup 属性 45
record caching 44
记录缓存 44
record caching, enabling 44
启用记录缓存 44
RecordViewCache 46
RecordViewCache 46
RecordViewCache instantiating, X++ select used 46
RecordViewCache 实例化，使用 X++ select 46
result-set caching 47
结果集缓存 47
result-set caching, limitations 47
结果集缓存，限制 47
types 44
类型 44
characteristics, Dynamics AX 4.0
特性，Dynamics AX 4.0
business processes, coherence 14
业务流程，协调性 14
code documentation 17
代码文档 17
comprehensive functionality 18
全面功能 18
flexibility 12
灵活性 12
integration 15
集成 15
internationalization 16
国际化 16
localization 16
本地化 16
scalable 17
可扩展 17
total cost of ownership (TCO) 12
总拥有成本 (TCO) 12
usability 13
可用性 13

**characteristics, ERP solution**
**特性，ERP 解决方案**
business processes, coherence 7
业务流程，协调性 7
code documentation 8
代码文档 8
flexibility 7
灵活性 7
integration 7
集成 7
internationalization 8
国际化 8
localization 8
本地化 8
modular 7
模块化 7
responsiveness 7
响应性 7
scalability 8
可扩展性 8
total cost of ownership (TCO) 6
总拥有成本 (TCO) 6
trustworthiness 7
可信度 7
usability 7
可用性 7
**classes, AOT object standards**
**类，AOT 对象标准**
about 60
关于 60
categories 60
类别 60
**code placement, application design standards**
**代码放置，应用程序设计标准**
AOT element type, guidelines 42, 43
AOT 元素类型，指南 42, 43
GUI objects 42
GUI 对象 42
methods 41
方法 41
methods, execution place 41
方法，执行位置 41
queries 42
查询 42
QueryRun, queries 42
QueryRun，查询 42
reports 42
报表 42
RunOn property 40
RunOn 属性 40
RunOn property, classes 40
RunOn 属性，类 40
temporary tables 42
临时表 42
three-tier architecture 40
三层架构 40
**code profiler, Dynamics AX**
**代码分析器，Dynamics AX**
call tree form, data viewing 141
调用树表单，数据查看 141
data, analyzing 141
数据，分析 141
data, gathering 140
数据，收集 140
data, viewing 141
数据，查看 141
invoking ways 140
调用方式 140
profile lines form, data viewing 141
分析行表单，数据查看 141
profile summary form, data viewing 143
配置文件摘要表单，数据查看 143
stages 140
阶段 140
totals form, data viewing 142
总计表单，数据查看 142
traverse form, data viewing 141
遍历表单，数据查看 141
uses 140
用途 140
**compare tool, Dynamics AX**
**比较工具，Dynamics AX**
comparison form 138
比较表单 138
comparison ways 138
比较方式 138
different application objects, comparing 140
比较不同的应用程序对象 140
same application object in two layers 138, 139
同一层中的相同应用程序对象 138, 139
version control system 139
版本控制系统 139
versions of same object, comparing 139
比较同一对象的版本 139
**compliance check to coding standards testing**
**符合编码标准检查测试**
about 96
关于 96
specifications 97
规范 97
**compliance check to design and architecture guidelines testing**
**符合设计和架构指南检查测试**
about 97
关于 97
specifications 97
规范 97
**criteria**
**标准**
definition 118
定义 118
entry criteria 114
入口标准 114
exit criteria 114
出口标准 114
resumption criteria 120
恢复标准 120
suspension criteria 119
暂停标准 119
test case, fail criteria 119
测试用例，失败标准 119
test case, pass criteria 119
测试用例，通过标准 119
**D**
**database design, performance optimization**
**数据库设计，性能优化**
caching 44
缓存 44
caching mechanism 45, 46
缓存机制 45, 46
forms, joins used 52
表单，使用连接 52
indexes, advantages 49
索引，优势 49
indexes, disadvantages 49
索引，劣势 49
indexes, types 48
索引，类型 48
indexing, rules 48
索引规则 48
indexing, tips 50
索引提示 50
record caching 44
记录缓存 44
RecordViewCache 46
RecordViewCache 46
RecordViewCache instantiating, X++ select used 46
RecordViewCache 实例化，使用 X++ select 46
result-set caching, about 47
结果集缓存，关于 47
result-set caching, limitations 47
结果集缓存，限制 47
select statement 51
select 语句 51
transactions 51
事务 51
**data dictionary, AOT object standards**
**数据字典，AOT 对象标准**
Base Enum 58
Base Enum 58
EDT 57
EDT 57
**defect**
**缺陷**
defect management system, requirements 129
缺陷管理系统，需求 129
defect management tool, requirements 129, 130
缺陷管理工具，需求 129, 130
defect management tool requirements, by IBI specifications 129
缺陷管理工具需求，按 IBI 规范 129
life cycle 130
生命周期 130
defect, Dynamics AX
缺陷，Dynamics AX
about 123
关于 123
bug priority 124
错误优先级 124
classification 124
分类 124
classification, factors 124
分类，因子 124
definition 123
定义 123
priority levels, in IBI specifications 124
优先级级别，在 IBI 规范中 124
security bugs 126
安全错误 126
severity 125
严重性 125
severity levels, in IBI specifications 125
严重性级别，在 IBI 规范中 125
defect management tool. See defect
缺陷管理工具。参见缺陷
disadvantages, ERP system
劣势，ERP 系统
business processes, reengineering 10
业务流程，重构 10
customization, limitations 10
定制，限制 10
rigidity 10
僵化 10
DoS attacks
DoS 攻击
CPU starvation 127
CPU 饥饿 127
memory starvation 127
内存饥饿 127
network starvation 127
网络饥饿 127
resource starvation 127
资源饥饿 127
types 127
类型 127
DREAD model
DREAD 模型
about 128
关于 128
security threat 128
安全威胁 128
Dynamics AX
Dynamics AX
about 11
关于 11
AOT object standards 57
AOT 对象标准 57
application design standards 39
应用程序设计标准 39
best practice check tool 133
最佳实践检查工具 133
Biztalk integration 15
Biztalk 集成 15
built-in tool 21
内置工具 21
characteristics 11
特性 11
characteristics, as an ideal ERP system 11
特性，作为理想的 ERP 系统 11
code profiler 32
代码分析器 32
code review 121, 122
代码审查 121, 122
compare tool 32
比较工具 32
customization 18
定制 18
customization files 24
定制文件 24
Dynamics AX Benchmark Toolkit 143
Dynamics AX Benchmark Toolkit 143
GUI standards 63
GUI 标准 63
layer system 21
层系统 21
module testing 113
模块测试 113
quality defining 27
质量定义 27
regression testing 114
回归测试 114
release testing 114
发布测试 114
RunOn property 40
RunOn 属性 40
shared standards 53
共享标准 53
stakeholders, responsibilities 120, 121
利益相关者，职责 120, 121
stakeholders, roles 120, 121
利益相关者，角色 120, 121
system testing 114
系统测试 114
testing 95
测试 95
testing, types 113
测试，类型 113
test life cycle 113
测试生命周期 113
trustworthy computing 86
可信计算 86
tools 133
工具 133
unit testing 113
单元测试 113
user-interface standards, objectives 63
用户界面标准，目标 63
user acceptance testing 114
用户验收测试 114
Dynamics AX, quality defining
Dynamics AX，质量定义
about 27
关于 27
back up 35
备份 35
coding standards, factors 27
编码标准，因子 27
coding standards, types 28
编码标准，类型 28
database upgrade script 37
数据库升级脚本 37
Dynamics AX best practice check tool, configuring 28
Dynamics AX 最佳实践检查工具，配置 28
Dynamics AX best practice check tool, deviations 28
Dynamics AX 最佳实践检查工具，偏差 28
Dynamics AX best practice check tool, severity levels 29
Dynamics AX 最佳实践检查工具，严重性级别 29
Dynamics AX best practice check tool, uses 28
Dynamics AX 最佳实践检查工具，用途 28
file versioning, for ActiveX controls 37
文件版本控制，用于 ActiveX 控件 37
file versioning, for DLL 37
文件版本控制，用于 DLL 37
globalization 32
全球化 32
IBI specification 31
IBI 规范 31
installation 34
安装 34
installation, difficulties 34
安装，困难 34
integration points, documentation 36
集成点，文档 36
localization 33
本地化 33
performance optimization 31
性能优化 31
platform compatibility 34
平台兼容性 34
quality assurance standards 29
质量保证标准 29
restore 35
恢复 35
setup 34
设置 34
setup, essential features 35
设置，基本功能 35
testing standards 29
测试标准 29
three-tier architecture 31
三层架构 31
trustworthiness, achieving 32
可信度，实现 32
