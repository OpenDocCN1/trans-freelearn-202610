

## Summary

我们介绍了 S4 类的概念并提供了若干示例。S4 类至少分两个阶段构建。第一阶段是定义类的名称和相关的数据组件。然后在单独的步骤中定义与该类相关的方法。

除了定义类及其方法之外，还探讨了继承的思想。本章给出了一个部分示例；它基于本章第一部分中定义的基础类。此外，还探讨了调用父类中相关方法的方法，并且示例使用了构造函数（或初始化方法）来演示如何构建构造函数链。

最后，解释了四个有用的命令。这四个命令提供了获取有关类或给定类对象的不同方式的信息。

如需了解更多信息，您可以参考 *Mobile Cellular Automata Models of Ant Behavior: Movement Activity of Leptothorax allardycei, Blaine J. Cole and David Cheshire, The American Naturalist.*

## Chapter 10. Case Study – Course Grades

本章是我们第一个案例研究。我们将前几章的思路汇集在一起并提供一个扩展示例。引入了一些新思想，在完整示例的背景下它们应该更容易理解。

本章大致分为四个部分：

- **The Course class**: 本节为您概述将包含课程成绩列表的 S4 类。成绩保存在一个列表中，每个评分任务都是一个单独的对象。
- **The assignment classes**: 本节为您概述用于保存特定评分任务成绩的 S4 类。此类有两个派生类。一个派生类用于跟踪具有数值分数的成绩，另一个用于跟踪由字母组成的成绩。
- **Extending existing functions**: 本节简要讨论了如何扩展现有函数以对属于 assignment 类的对象做出适当反应。我们重点关注 summary、plot 和 show 命令。
- **Extending operations**: 本节讨论了如何通过构建在现有方法之上来扩展算术和访问函数。

### Overview

所有前几章都专注于特定主题，而在这里我们将多个不同主题汇集起来以检查一个扩展示例。这里检查的所有类都是 S4 类。这些类用于读取包含班级学生成绩的 CSV 文件。其中一个类定义了如何读取成绩文件以及如何解释一列。另一组类被定义并且这些类用于跟踪单个作业的信息。此类的一个对象将包含所有学生的作业所有分数。

我们首先讨论新的 Course 类，它用于跟踪所有作业。接下来，我们讨论用于跟踪单个作业成绩的 assignment 类。一旦类被定义，就会给出扩展 summary、plot 和 show 命令的细节，以演示如何扩展常用命令以便在传递新定义的对象时做出反应。最后，重新定义了一些基本的算术和访问运算符，以便 R 系统在使用熟悉的操作（如加法或乘法）时做出适当反应。

Course 类包含其类型为 assignment 类之一的对象，似乎更自然地先定义 assignment 类。assignment 类的行为更复杂且包含更多细节，而 Course 类相对简单。为了让类介绍更温和，我们将先讨论 Course 类。

### The Course class

Course 类用于跟踪课程的所有成绩。此类将从文件中读取信息，判断成绩是数值成绩还是字母成绩，并创建适当的 assignment 对象来保存成绩。类的细节如以下图所示。我们将先讨论数据，然后讨论与该类相关的方法。然后我们将给出如何定义类的一些细节。出于简洁考虑，与访问器相关的代码大部分被省略，但完整代码可在本书相关网站上获得。

![](img/18eeabe9ab8c81c5a3f4040db5906c67_215_0.png)

首先，该类有三个数据结构（slots）。第一个是包含班级所有成绩的 CSV 文件的名称。第二个是用于确定文件中有哪些类型的评分任务的向量前缀。最后一个是包含所有作业的列表。

共有七个方法。除了一个之外，其余都用于设置和获取三个 slots 的值。获取和设置文件名的访问器例程在此给出，而其他例程未在此文中包含。最后一个例程用于读取成绩，这是一个复杂的方法，它从文件中读取数据，然后判断作业是否有数值或字母成绩。然后它定义一个适当的 assignment 对象。

### The definition of the Course class

给出 Course 类的定义，然后说明 slots 的访问器。这是一个 S4 类。在本例中，我们不提供任何检查以确保 slots 中的信息一致，以降低示例的复杂性：

```
###########################################
# Create the Course class to keep track of all grades
Course <- setClass(
    # Set the name for the class
    "Course",

    # Define the slots
    slots = c(
        GradesFile = "character",
        GradeTypes = "character",
        Grades     = "list"
    ),

    # Set the default values for the slots. (optional)
    prototype=list(
        GradesFile = "",
        GradeTypes = c("test","hw","quiz","project"),
        Grades     = list()
    ),

    # Make a function that can test to see if the data is consistent.
    # This is not called if you have an initialize function defined!
    validity=function(object)
    {
        return(TRUE)
    }
)
```

每个 slots 都包含一组访问器函数，以帮助检索或设置类跟踪的信息。我们只提供其中一组访问器的细节。设置和获取文件名的 methods 给出。默认 R 环境中不存在这些方法，因此必须先创建，如下所示：

```
# Define the methods used to retrieve or set the values within a Course object.
setGeneric(name="GetFileName",
           def=function(course)
           {
               standardGeneric("GetFileName")
           }
)

setMethod(f="GetFileName",
          signature="Course",
          definition=function(course)
          {
              return(course@GradesFile)
          }
)
```

### Reading grades from a file

最后检查的方法是从给定文件读取数据的方法。在提供列表之前，我们将先检查 `Course` 类中的 `Grades` 列表。此 slot 是一个列表，列表中的每个元素都是一个 assignment 对象。 （assignment 类在下一节中讨论。）列表中对象的名称与 CSV 文件第一行中找到的名称相同。

`ReadGrades` 方法首先读取 `csv` 文件。然后它遍历从文件中读取的列，并且假设列名在文件第一行中。如果列名的第一个字母与 `GradeTypes` slot 中的向量中的一个字符串匹配，则假设该列代表一个评分项目。有两种类型的作业：数值成绩或字母成绩。如果从数据文件中确定某列为评分项目，则检查其类型。如果类型为数值类型，则假设课程类型为数值成绩（`NumericGrade` 类的对象）；否则，假设为字母成绩（`LetterGrade` 类的对象）：

```r
setGeneric(name="ReadGrades",
           def=function(course)
           {
               standardGeneric("ReadGrades")
           }
)
```

```r
setMethod(f="ReadGrades",
          signature="Course",
          definition=function(course)
          {
              grades <- read.csv(GetFileName(course))
              convertedGrades <- list()
              courseTypes <- GetGradeTypes(course)
              for (gradeItem in names(grades))
              {
                  # Go through each column from the file.
                  for (type in courseTypes)
                  {
                      # go through each course type and determine if
                      # this column is a quiz/test/hw?
                      if(length(grep(type,gradeItem))>0)
                      {
                          # The prefix for the name matches one of
                          # the predefined types.
                          if((class(grades[[gradeItem]])=="numeric" ||
                             (class(grades[[gradeItem]])=="integer")) {
                              thisItem <- NumericGrade()
                              #print(paste("This item,",gradeItem,
                              #   ",is a numeric grade item.",
                              #   class(thisItem)))
                          } else {
                              thisItem <- LetterGrade()
```

