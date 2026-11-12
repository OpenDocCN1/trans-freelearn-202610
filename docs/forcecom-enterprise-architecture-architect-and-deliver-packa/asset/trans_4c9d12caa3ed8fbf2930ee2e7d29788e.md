

通过使用 Apex 接口和通用服务，我们按如下方式分离合规功能实现的功能关注点：

- 服务层代码仅关注通过接口执行合规检查过程
- 控制器代码负责调用服务类方法执行合规检查，然后以一致的方式向用户呈现结果
- 实现该接口的领域层类仅专注于根据对象类型，验证记录数据是否符合适用规则

有了这种关注点分离，就可以轻松显著修改或扩展检查的调用方式和向用户呈现的方式，而无需重新访问每个领域层类；或者，可以修改某个对象的合规逻辑，而不会影响整个应用程序的总体实现。

## Apex 接口示例

接口有助于表达不同领域类之间的共同属性或行为。在本节中，我们将使用 Apex 接口来帮助暴露每个适用领域类中封装的合规检查逻辑。

为了练习和实现合规框架，FormulaForce 应用程序增加了一些额外的字段和对象。这些对象、附加类和 Visualforce 页面均包含在本章的代码示例中。

总结一下，为实现合规框架要求，已采取以下步骤。您可以跟随这些步骤操作，也可以直接参考本章的示例代码：

- 1. 创建新的 Car 对象和选项卡。
- 2. 在 Car 对象上创建类型为 Number、长度为 6 的新 Weight 字段。
- 3. 向 Driver 对象添加新的 FIA Super License 字段。
- 4. 创建新的 Cars 领域类和 CarsSelector 选择器类。
- 5. 创建新的 ComplianceService 类和 ICompliant 接口。
- 6. 创建新的 Cars 和 Drivers 领域类并实现 ICompliant 接口。
- 7. 更新 Application 类以实现领域类工厂
- 8. 创建利用领域工厂的新 ComplianceService.verify 方法。
- 9. 创建用户界面以从实现该接口的各种对象访问服务。

> 最后一步将创建两个用户界面，一个用于 Salesforce Classic，另一个用于 Lightning Experience。ComplianceController 类为每个对象的每个特定 Visualforce 页面提供通用控制器类。此外，ComplianceChecker 包提供了一个名为 Compliance Checker 的通用 Lightning 组件，可以放置在任何对象页面上。

以下各部分将从步骤 5 开始更详细地描述 Apex 代码。

## 第 5 步 – 定义通用服务

目标是开发一个通用服务来处理整个应用程序中的所有合规验证。因此，创建了一个带有 verify 方法的新 ComplianceService 类，该方法能够接收来自各种对象的 ID。如果有任何验证错误，它们将作为异常的一部分抛出。如果该方法在不抛出异常的情况下返回，则给定的记录是合规的。以下代码片段展示了带有 verify 方法的新 ComplianceService 类的创建：

```
public class ComplianceService {
/**
 * Provides general support to verify compliance in the
 * application
 * @throws ComplianceException for any failures
 */
public static void verify(Set<Id> recordIds) {
// Query the given records and delegate to the
// corresponding Domain class to check compliance
// and report failures via ComplianceException
}
```

在深入了解如何实现此服务方法之前，请先看看新的 `ICompliant` 接口以及将由 Drivers 和 Cars 领域类实现的 `verifyCompliance` 方法：

```
public class ComplianceService {
/**
 * Interface used to execute compliance checking logic
 * in each domain class
 */
public interface ICompliant {
List<VerifyResult> verifyCompliance();
}

/**
 * Results of a compliance verification for a given record
 */
public class VerifyResult {
public Id recordId;
public String complianceCode;
public Boolean passed;
public String failureReason;
}
}
```

## 第 6 步 – 实现领域类接口

`Drivers` 领域类按如下方式实现该接口（仅显示部分类）。它检查 FIA Super License 字段是否已选中，如果未选中，则报告相应的合规代码和失败消息：

```
public class Drivers extends ApplicationDomain
implements ComplianceService.ICompliant {
public List<ComplianceService.VerifyResult> verifyCompliance() {
List<ComplianceService.VerifyResult> compliances =
new List<ComplianceService.VerifyResult>();
for(Driver__c driver : (List<Driver__c>) Records) {
ComplianceService.VerifyResult license =
new ComplianceService.VerifyResult();
license.ComplianceCode = '4.1';
license.RecordId = driver.Id;
license.passed = driver.FIASuperLicense__c;
license.failureReason =
license.passed ? null :
'Driver must have a FIA Super License.';
compliances.add(license);
}
return compliances;
}
}
```

Cars 领域类（仅显示部分类）按如下方式实现 verifyCompliance 方法，检查汽车是否超过指定重量：

```
public class Cars extends ApplicationDomain
    implements ComplianceService.ICompliant {
public List<ComplianceService.VerifyResult> verifyCompliance() {
    List<ComplianceService.VerifyResult> compliances =
        new List<ComplianceService.VerifyResult>();
    for(Car__c car : (List<Car__c>) Records) {
        // Check weight compliance
        ComplianceService.VerifyResult license =
            new ComplianceService.VerifyResult();
        license.ComplianceCode = '4.1';
        license.RecordId = car.Id;
        license.passed =
            car.Weight__c !=null && car.Weight__c>= 691;
        license.failureReason = license.passed ? null :
            'Car must not be less than 691kg.';
        compliances.add(license);
    }
    return compliances;
}
}
```

## 第 7 步 – 领域类工厂模式

下一步是根据传入 `ComplianceService.verify` 方法的记录 ID，动态构建关联的领域类。实现的这一部分使用了领域类的工厂模式实现。

> 关于工厂模式的 Wikipedia 参考可在 http://en.wikipedia.org/wiki/Factory_method_pattern 上找到。

实现使用 Apex 运行时 `Id.getSObjectType` 方法来确定 `SObjectType`，然后通过以下代码所示的 `Maps`，确定要使用的适用领域类和选择器类（以查询记录）。

然后使用 Apex 运行时 `Type.newInstance` 方法来构建领域类和选择器类的实例。通过调用 `Application.Domain.newInstance` 方法来调用工厂功能。Application 类上的 `Domain` 字段使用 `fflib_Application.DomainFactory` 类按如下方式实现：

```
public class Application {
  public static final fflib_Application.DomainFactory Domain =
    new fflib_Application.DomainFactory(
      Application.Selector,
      // Map SObjectType to Domain Class Constructors
      new Map<SObjectType, Type> {
        Race__c.SObjectType => Races.Constructor.class,
        Car__c.SObjectType => Cars.Constructor.class,
        Driver__c.SObjectType => Drivers.Constructor.class
      });
}
```

> 作为开发人员，您当然可以直接实例化特定的领域类。但是，这种领域类工厂方法允许根据传递给 newInstance 方法的 ID 关联的 SObjectType 在运行时选择特定的领域类。这是实现下一步所示的通用服务入口点的关键。领域类工厂的前面初始化还传入了 Application.Selector；这是另一个与能够查询数据库的类相关的工厂。下一章将更详细地介绍选择器模式和选择器工厂。

## 第 8 步 – 实现通用服务

以下代码中使用的 `Application.Domain.newInstance` 方法用于访问上一步中定义的领域类实例工厂。

该方法返回 `fflib_ISObjectDomain`，因为这是所有领域类的共同基接口。然后，使用 `instanceof` 运算符，它确定是否实现了 `ICompliant` 接口，如果是，则调用 `verifyCompliance` 方法。以下代码展示了通用服务的实现：

```
public class ComplianceService {
  public static void verify(Set<Id> recordIds) {
    // Dynamically create Domain instance for these records
    fflib_ISObjectDomain domain =
      Application.Domain.newInstance(recordIds);
    if (domain instanceof ICompliant) {
      // Ask the domain class to verify its compliance
      ICompliant compliantDomain = (ICompliant) domain;
```

