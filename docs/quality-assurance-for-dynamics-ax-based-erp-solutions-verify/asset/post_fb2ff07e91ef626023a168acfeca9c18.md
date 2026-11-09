
methods and the lower part (again divided into two sections) shows the child and parent calls for the method selected. To select a method, double-click on it and it will show you child and parent calls in the lower section.

To limit the type of call shown the following buttons can be used:

| Button | Type of calls |
|---|---|
| Profile line | Opens the profile lines form to show all lines of code that are executed. |
| Setup | This has four items to limit the type of calls displayed
None: No filtering, view all fields.
Code hot spots: View only the Duration, Share – duration, Method calls, and Share – method calls fields.
Database access: View only the Method calls, Share – method calls, and Calculated SQL time fields.
Remote connection: View only the Method calls, Share – method calls, and Calculated AOS time fields. |

## Totals Forms

The **Totals** forms are used to display aggregated data. Various **Totals** forms can be accessed from **Tools | Development tools | Code profiler | Profiler runs | Totals.**

Line total form: Displays aggregated data about total amount of time spent on executing each line of code for a code profiler run.

Method total form: Displays aggregated data about total amount of time spent on executing each method for a code profiler run.

Object total form: Displays aggregated data about total amount of time spent on executing each object for a code profiler run.

Following is a summary of the buttons available on each of the three forms:

| Button name and Function | Line total form | Method total form | Object total form |
|---|---|---|---|
| Profile lines: Opens Profile lines form | Yes | Yes | No |
| Line total: Opens Line total form | No | Yes | Yes |
| Method total: Opens Method total form | No | No | Yes |

## Profile Summary Form

This form is used to calculate the total execution time for each line of code, each method, and each object for a code profiler run. This form is used when the 'Calculate line total' checkbox is cleared on the Code profiler form while creating a run; else total calculation is automatically done.

> The code profiler tool will be soon replaced by "Trace Parser" trace parser.

## Dynamics AX Benchmark Toolkit

The 'Dynamics AX Benchmark Toolkit' for performance testing is an external tool supplied along with **Microsoft Dynamics AX Resource Kit**. It offers the following distinct advantages in comparison to the benchmark tool in Axapta 3.0.

- Greater functional coverage.
- Light-weight business connectors in comparison to Dynamics AX clients.
- 'Dynamics AX Benchmark Toolkit' is a managed assembly in C#. It has two major libraries, i.e. The Programming Model Library and The Microsoft Dynamics AX Wrapper Library. The first library is to provide a gateway to access Dynamics AX through business connectors. The latter library (Microsoft Dynamics AX Wrapper) is to generate type-safe managed classes to interface with Microsoft Dynamics AX. The prerequisite for learning about how to use this tool is to know .NET and X++ so this tool is not being discussed in this book.

## Summary

We discussed some Dynamics AX built-in tools, which may play a vital role in making the testing process more efficient and effective. Dynamics AX also has a Unit-test framework, which is beyond the scope of this book, as unit testing is usually carried out by the development team itself.

## Index

A
Access Control List. See ACL
ACL
about 86
tips 86
advantages, ERP system
management control 9
operational control 9
strategic planning 10
AIF. See Application Integration framework
AOT
element 34
application design standards, Dynamics AX
Auto property settings 53
code placement 39
field groups using, in table 52
performance optimization 43
Application Integration Framework 15
Application Object Tree. See AOT
authorization testing
about 102
specifications 102, 103
SysBPCheckIgnore Macro 137
bug
priority 124
security bugs 126
B
back up and restore testing
about 107
specifications 107
best practice check tool, Dynamics AX
applicable to all AOT elements 134
applicable to specific AOT elements
135-137
BP Deviation documented, comment 137
category information 134
invoking manually 133
C
caching
about 44
CacheLookup property 45, 46
EntireTable, CacheLookup property 46
Found, CacheLookup property 45
FoundAndEmpty, CacheLookup property 45
mechanism 45, 46
None, CacheLookup property 45
NotITTS, CacheLookup property 45
record caching 44
record caching, enabling 44
RecordViewCache 46
RecordViewCache instantiating, X++ select used 46
result-set caching 47
result-set caching, limitations 47
types 44
characteristics, Dynamics AX 4.0
business processes, coherence 14
code documentation 17
comprehensive functionality 18
flexibility 12
integration 15
internationalization 16
localization 16
scalable 17
total cost of ownership (TCO) 12
usability 13

**characteristics, ERP solution**
business processes, coherence 7
code documentation 8
flexibility 7
integration 7
internationalization 8
localization 8
modular 7
responsiveness 7
scalability 8
total cost of ownership (TCO) 6
trustworthiness 7
usability 7
**classes, AOT object standards**
about 60
categories 60
**code placement, application design standards**
AOT element type, guidelines 42, 43
GUI objects 42
methods 41
methods, execution place 41
queries 42
QueryRun, queries 42
reports 42
RunOn property 40
RunOn property, classes 40
temporary tables 42
three-tier architecture 40
**code profiler, Dynamics AX**
call tree form, data viewing 141
data, analyzing 141
data, gathering 140
data, viewing 141
invoking ways 140
profile lines form, data viewing 141
profile summary form, data viewing 143
stages 140
totals form, data viewing 142
traverse form, data viewing 141
uses 140
**compare tool, Dynamics AX**
comparison form 138
comparison ways 138
different application objects, comparing 140
same application object in two layers 138, 139
version control system 139
versions of same object, comparing 139
**compliance check to coding standards testing**
about 96
specifications 97
**compliance check to design and architecture guidelines testing**
about 97
specifications 97
**criteria**
definition 118
entry criteria 114
exit criteria 114
resumption criteria 120
suspension criteria 119
test case, fail criteria 119
test case, pass criteria 119
**D**
**database design, performance optimization**
caching 44
caching mechanism 45, 46
forms, joins used 52
indexes, advantages 49
indexes, disadvantages 49
indexes, types 48
indexing, rules 48
indexing, tips 50
record caching 44
RecordViewCache 46
RecordViewCache instantiating, X++ select used 46
result-set caching, about 47
result-set caching, limitations 47
select statement 51
transactions 51
**data dictionary, AOT object standards**
Base Enum 58
EDT 57
**defect**
defect management system, requirements 129
defect management tool, requirements 129, 130
defect management tool requirements, by IBI specifications 129
life cycle 130
defect, Dynamics AX
about 123
bug priority 124
classification 124
classification, factors 124
definition 123
priority levels, in IBI specifications 124
security bugs 126
severity 125
severity levels, in IBI specifications 125
defect management tool. See defect
disadvantages, ERP system
business processes, reengineering 10
customization, limitations 10
rigidity 10
DoS attacks
CPU starvation 127
memory starvation 127
network starvation 127
resource starvation 127
types 127
DREAD model
about 128
security threat 128
Dynamics AX
about 11
AOT object standards 57
application design standards 39
best practice check tool 133
Biztalk integration 15
built-in tool 21
characteristics 11
characteristics, as an ideal ERP system 11
code profiler 32
code review 121, 122
compare tool 32
customization 18
customization files 24
Dynamics AX Benchmark Toolkit 143
GUI standards 63
layer system 21
module testing 113
quality defining 27
regression testing 114
release testing 114
RunOn property 40
shared standards 53
stakeholders, responsibilities 120, 121
stakeholders, roles 120, 121
system testing 114
testing 95
testing, types 113
test life cycle 113
trustworthy computing 86
tools 133
unit testing 113
user-interface standards, objectives 63
user acceptance testing 114
Dynamics AX, quality defining
about 27
back up 35
coding standards, factors 27
coding standards, types 28
database upgrade script 37
Dynamics AX best practice check tool, configuring 28
Dynamics AX best practice check tool, deviations 28
Dynamics AX best practice check tool, severity levels 29
Dynamics AX best practice check tool, uses 28
file versioning, for ActiveX controls 37
file versioning, for DLL 37
globalization 32
IBI specification 31
installation 34
installation, difficulties 34
integration points, documentation 36
localization 33
performance optimization 31
platform compatibility 34
quality assurance standards 29
restore 35
setup 34
setup, essential features 35
testing standards 29
three-tier architecture 31
trustworthiness, achieving 32
