

## 其他依赖注入方式

构造函数模式的一种变体是使用设置器方法将引用注入到正在进行单元测试的类的实例中。这些方法应使用 @TestVisible 注解，以便这些方法不会被用于覆盖实例设置来指向依赖类的真实实例（这些实例可能在构造函数中设置）。

本书前面讨论的工厂模式也可用于注入模拟实现的替代实现。工厂方法不要求为每个类的构造函数声明依赖项列表。相反，通过调用工厂方法来获取接口的实例。

> 使用工厂依赖注入而非 CDI 是否偏好，取决于是否需要前置声明依赖项。Apex Enterprise 模式库选择使用工厂模式而非 CDI 进行注入，因为服务、域和选择器等层在整个应用程序代码库中定义良好且应用一致。

本书前面章节中使用的 Application 类工厂允许你通过调用每个相应工厂上的相应方法，来实现服务、域或选择器实现的运行时注入。

以下 Application 类工厂方法允许你获取实现指定服务、工作单元、域或选择器类的对象实例。这些是前面章节中用于在运行时实现各种动态配置行为的相同方法：

```
Application.Service.newInstance
Application.Domain.newInstance
Application.Selector.newInstance
Application.UnitOfWork.newInstance
```

必须将结果强制转换为适用的 Apex 类或接口。

以下方法可用于在给定服务、域或选择器类的测试设置期间注册替代实现。

```
Application.Service.setMock
Application.Domain.setMock
Application.Selector.setMock
Application.UnitOfWork.setMock
```

前面的 newInstance 方法随后将优先返回这些实例，而不是你在 Application 类中配置的真实类的实例。

在前面一章中，RaceService 已创建并实现了 IRaceService。以下 RaceController 类的单元测试利用此接口和 Application.Service.setMock 方法。要向工厂注册 IRaceService 接口的模拟实现。为了实现一个验证控制器的记录 ID 是否正确传递到所调用服务方法的单元测试：

```
@IsTest
private static void whenAwardPointsCalledIdPassedToService() {

    // Given
    MockRaceService mockService = new MockRaceService();
    Application.Service.setMock(IRaceService.class, mockService);

    // When
    Id raceId = fflib_IDGenerator.generate(Race__c.SObjectType);
    RaceController raceController =
        new RaceController(
            new ApexPages.StandardController(
                new Race__c(Id = raceId)));
    raceController.awardPoints();

    // Then
    System.assert(mockService.awardChampionshipPointsCalled);
    System.assertEquals(new Set<Id> { raceId },
        mockService.awardChampionshipPointsCalledWithRaceIds);
}
```

> 为简洁起见，此处未显示 MockRaceService，但它遵循与本章前面所示的其他模拟类相同的模式。请参阅本章示例代码中包含的 RaceControllerTestMockingDemo 类中的相关内容。另请回想，RaceService.service 方法使用 Application 服务工厂来创建要执行的服务实例。

## 依赖注入框架的优势

迄今为止，我们已经讨论了通过使用工厂或构造函数并实现 Apex 接口来注入要模拟的类的替代模拟实现，从而实现依赖注入的方法。DI 框架为开发人员提供了访问某些底层语言运行时功能的方式，这些功能可以提供更直接的方式来实现这些方面。

因此，DI 框架倾向于帮助开发人员以更少的样板代码（例如使用接口）更轻松地获得 DI 的好处，尤其是在旧代码库中。然而，开发人员仍可能需要编写如上所见的模拟实现，并通过 SOC 仔细管理类之间的依赖关系。

例如，Java 利用其反射 API 允许在运行时进行注入，而无需接口。Java Spring 框架通过允许通过 XML 文件进行配置来扩展此功能，以允许不同的运行时配置。某些框架还会拦截原生 *new* 运算符的使用。这避免了实现前面代码中概述的基于工厂或构造函数的注入方法的需要。

Apex 运行时的相对较新的补充是 **Apex Stub** API。这是 Salesforce 作为其平台一部分提供的此类 DI 框架之一。它不覆盖 *new* 运算符。开发人员仍需要实现如前所述的注入方法。然而，它确实使在旧代码库中（未应用接口的地方）采用模拟变得更加容易。本章剩余部分将继续更详细地探索此 API，并且将在其之上构建框架。

## 使用 Apex Stub API 编写单元测试

**Apex Stub** API 仅适用于 Apex 测试上下文。因此，它不能用于在测试之外实现 DI。为此，您仍需利用 Apex 接口。

利用这些 API 需要了解以下内容：

- 实现 Stub 提供程序接口：系统提供的 `System.StubProvider` Apex 接口实际上是一种回调式接口。它允许您的模拟代码在测试中针对正在模拟的类进行方法调用时得到通知。您可以多次实现此接口，每个模拟类一次，或者使用单个实现来构建复杂的通用模拟框架。FinancialForce.com 的 Apex Mocks 开源框架就是一个我们将在后面审查的此类框架。

- 模拟的动态创建存根：平台会通过 Test.createStub 方法自动创建您希望模拟的类的实例。使用 Stub API 时，您不需要创建 Apex 接口来执行 DI。唯一的要求是向平台提供 Test.StubProvider 接口的实现，以便在测试执行期间调用创建的存根实例上的方法时进行回调。

## 使用 Test.StubProvider 实现模拟类

使用 System.StubProvider 实现模拟处理代码只需实现一个方法，而无需逐个实现正在模拟的每个方法。这是因为此接口是所有可能方法签名的通用接口。

如以下 MockDashboard 类的实现所示，此接口上的 handleMethodCall 方法会传递模拟代码所需的所有信息，以识别哪个对象的哪个方法已被调用。

```
private class MockDashboard implements System.StubProvider {

    public Boolean initialiseCalled = false;

    public Object handleMethodCall(
        Object stubbedObject,
        String stubbedMethodName,
        Type returnType,
        List<Type> listOfParamTypes,
        List<String> listOfParamNames,
        List<Object> listOfArgs) {

        // Record method call
        if(stubbedMethodName == 'initialise') {
            initialiseCalled = true;
        }

        return null;
    }
}
```

再次强调，此模拟实现仅实现了满足所执行测试的模拟要求所需的最低限度。它只验证 initialise 方法是否被调用。

> Salesforce 提供了一组丰富的参数，以允许对此方法进行更通用的实现。我们将在本章后面通过 Apex Mocks 框架审查这些参数。

## 为模拟类创建动态存根

实现 System.StubProvider 后，仍无法将 MockDashboard 类的实例传递给 Car 类，因为它无法强制转换为 dashboard 的实例。要使此成为可能，必须调用平台提供的 Test.createStub 方法。这在内部创建 Dashboard 类的实例，但将方法调用路由到提供的存根提供程序，而不是实际代码。

我决定在这些示例中遵循的一种约定是，通过模拟类上的自己的 createStub 方法来封装对此方法的调用。这减少了调用它时的重复量（样板代码），并在编写单元测试代码时更易于使用，如您将在下一节中看到。

```
private class MockDashboard implements System.StubProvider {
    public Boolean initialiseCalled = false;

    public Object handleMethodCall(... ) {
    ...
    }

    public Dashboard createStub() {
    return (Dashboard) Test.createStub(Dashboard.class, this);
    }
}
```

## 使用 Apex Stub API 的模拟示例

实现 MockDashboard 类后，我们可以在单元测试中使用它。生成的单元测试类似于使用 Apex 接口所示的测试：

```
@IsTest
private static void
whenCarStartCalledDashboardAndEngineInitialised() {
// Given
MockDashboard mockDashboard = new MockDashboard();

