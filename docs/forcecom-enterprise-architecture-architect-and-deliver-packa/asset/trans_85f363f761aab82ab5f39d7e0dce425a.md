

正如我们在上一章中所识别的，处理批量记录集是一个常见需求。事实上，它存在于您的代码路径中，因为代码中任何层级循环内的 DML 或 SOQL 都有触及 governor 限制的风险。

因此，在设计 Service 层的方法时，默认考虑列表参数是合适的。这鼓励在方法中开发批量化代码，并避免调用者在循环中调用 Service 方法。

以下是非批量化代码的示例：

```
RaceService.awardChampionShipPoints(Id raceId)
```

以下是另一个非批量化代码的示例：

```
RaceService.awardChampionShipPoints(Set<Id> raceIds)
```

例如，带有多个参数的非批量化方法可能如下所示：

```
RaceService.retireFromRace(Id raceId, String reason)
```

前述方法签名的批量化版本可以利用前文所述的 Apex 内部类，如 *_定义和传递数据_* 小节中的以下示例所示。

> 有时，实现服务方法的批量化版本所需的成本和复杂性可能高于调用者实际需要的程度，因此这不应被视为固定准则，但至少应始终予以考虑。

## 共享规则强制执行

如前章所讨论，默认情况下 Apex 代码在系统模式下运行，意味着不强制执行共享规则。但是，业务逻辑行为一般应当遵守共享规则。为避免共享用户无权访问的信息，共享规则强制执行必须是 Service 层关注的问题。

Salesforce 安全审查要求 Apex 控制器类入口点遵守此规则，尽管您的 Service 层将由这些类调用，从而可能继承此上下文。请记住，您的 Service 层实际上是其他访问点和集成的入口点（我们将在后续章节和全书中探讨）。

因此，Service 层的默认关注点应当是强制执行共享规则。在 Service 层内实现或由其调用的代码应当继承此行为。只有在需要时，代码才应提升到运行在忽略共享规则的上下文中，否则称为 `without sharing` 上下文。这种情况发生在服务代表用户处理记录时。例如，服务可能会计算或汇总一些赛事数据，但某些原始赛事数据记录（来自其他赛事）可能对用户不可见。

要在 Service 层内默认强制执行共享规则，需在类定义中使用 with sharing 关键字，如下所示：

```
public with sharing class RaceService
```

您创建的其他 Apex 类，包括我们将讨论的 Selector 和 Domain 模式相关的类，应将其留空（未指定），以便它们继承上下文。这样可以让它们更轻松地在任一上下文中复用。

如果需要 without sharing 上下文，可以使用如下示例所示的私有内部类方法，临时提升执行上下文以在此模式下处理查询或 DML 操作：

```
// Class used by the Service layer
public class SomeOtherClass {

// Work in this method inherits with sharing context from
Service
public static void someMethod {
// Do some work in inherited context
// ...

// Need to do some queries or updates in elevated context
new ElevatedContext().restOfTheWork(workToDo);
}

private void restOfTheWork(List<SomeWork> workToDo) {
// Additional work performed by this class
// ...
}

private without sharing class ElevatedContext {
public void restOfTheWork(List<SomeWork> workToDo) {
// Do some work in a elevated (without sharing) context
SomeOtherClass.restOfTheWork(workToDo);
}
}
}
```

请注意，如果您认为某些调用者会想要禁用此强制执行，可以考虑将运行逻辑的能力作为 Service 层的参数。在本例中，上述代码示例可以改为直接或通过 ElevatedContext 内部类有条件地执行 restOfWork 方法。

> 在决定何时强制执行和何时不执行时，对象级和字段级安全也是一个重要考量。后续聚焦于 Domain 和 Selector 模式的章节将进一步讨论此话题。

## 定义和传递数据

在定义 Service 层与其调用者之间交换的数据时，请牢记 Service 层的职责是对调用者保持中立。除非您明确为这些数据格式开发功能，否则应避免通过 JSON 或 XML 字符串返回信息；请让调用者（例如，JavaScript remoting 控制器）处理此类数据编组要求。

根据指南，使用内部类是表达和限定服务方法所用数据结构的好方法。以下代码还说明了前一节中所示的多参数非批量化方法的批量化版本。

> 以这种方式思考使用内部类，也是解决原始类型痴迷（primitive obsession）症状的一个好方法（http://c2.com/cgi/wiki?PrimitiveObsession）。

请查看以下代码：

```
public class ContestantService {

    public class RaceRetirement {
        public Id contestantId;
        public String reason;
    }

    public static void retireFromRace(List<RaceRetirement> retirements) {
        // Process race retirements...
    }
}
```

尝试通过在服务之间复用内部类来避免过多的服务耦合。如果开始出现这种情况，这可能表明您可能需要一个新的共享或通用服务。

请始终牢记，服务方法实际上只需要完成其工作所需的最少信息，并通过方法签名和相关类型来表达这一点，以便调用者能清晰地看出只需要或仅返回该信息。这避免了调用代码中的疑虑和混淆，从而防止传递过少或冗余的信息。

前一个示例在 RaceRetirement 类中使用了读写成员变量，表明两者都是必需的。RaceRetirement 类的内部类仅用作此方法的输入参数。在将内部类同时用作输入和输出类型之前，请予以考虑，因为在此类类中，哪些成员变量应针对输入用例填充、哪些会在输出情况下填充并不总是清晰明了。

但是，如果您发现有这样的需求，即同一 Apex 类型既用作输出参数又用作输入参数，且某些信息在输入时并非必需，您可以考虑通过将属性设为只读来使用 Apex 属性语法来指示这一点。这可防止调用者不必要地填充成员字段的值。例如，请考虑 RaceService 中的以下服务方法：

```
public static Map<Id, List<ProvisionalResult>> calculateProvisionalResults(Set<Id> raceIds)
{
 // Implementation
}
```

```
public static void applyRaceResults(Map<Id, List<ProvisionalResult>> provisionalResultsByRaceId)
{
 //Implementation
}
```

```
public class ProvisionalResult {
 public Integer racePosition {get; set;}
 public Id contestantId {get; set;}
 public String contestantName {get; private set;}
}
```

在调用 calculateProvisionalResults 方法时，contestantName 字段作为便利项被返回，但被标记为只读，因为在 applyRaceResults 方法的上下文中不需要它。

## 在 Service 层接口中使用 SObject 时的注意事项

在以下示例中，Service 方法看起来将 Race 记录作为输入，这要求调用者查询对象并决定查询哪些字段。这是 Service 方法与调用者之间的宽松契约定义：

```
RaceService.awardChampionShipPoints(List<Race__c> races)
```

对调用者而言，更好的契约是只索要所需内容，在以下示例中即为 ID：

```
RaceService.awardChampionShipPoints(Set<Id> raceIds)
```

尽管可能会传递与不同对象类型记录相关的 ID（有人可能会考虑执行一些参数验证来拒绝此类列表），但这是一个更具表达性的契约，聚焦于服务真正需要的内容。调用者现在知道只会使用 ID。

在第一个示例中，当使用 Race__c SObject 类型作为参数类型时，不清楚调用者需要填充哪些字段，这可能使代码变得脆弱，因为这无法在接口定义中表达。此外，服务代码中所需的字段可能会更改，并要求调用者逻辑进行重构。这也可能是 Service 层内业务逻辑未能妥善处理封装问题的一个示例。

可以说，这种设计方法会带来额外的开销，尤其是当调用者已为展示目的查询了 Race 记录，而服务随后必须重新查询信息时。但是，在这种情况下，保持封装和复用问题可能是更有说服力的理由。仔细监控此注意事项可以使服务接口更清晰、封装性更好，最终更加健壮。

