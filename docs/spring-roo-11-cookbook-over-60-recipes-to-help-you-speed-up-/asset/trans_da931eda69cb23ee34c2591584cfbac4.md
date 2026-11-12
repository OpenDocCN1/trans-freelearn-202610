
与 @RooFlexScaffold 注释对应的 AspectJ ITD 文件的名称遵循以下命名约定：<JPA-entity-name>Service_Roo_Service.aj，其中 <JPA-entity-name> 是 @RooFlexScaffold 注释的 entity 属性所指定的 JPA 实体的名称。

从 Flex 客户端调用 Spring 管理的 remoting 目标方法可能需要发送和接收对象。例如，FlightDescriptionService 的 show 方法返回一个 FlightDescription 对象，而 create 方法接受一个 FlightDescription 对象。Flex 允许通过自动将 ActionScript 对象转换为 Java 对象（反之亦然）来在 Flex 客户端和 remoting 目标方法之间交换数据。由于 flightapp_flex 应用程序中的 Flex 客户端与 FlightDescriptionService remoting 目标交换航班描述详细信息，Roo 会生成与 FlightDescriptionService 管理的 JPA 实体对应的 ActionScript 类。以下代码显示了与 FlightDescription JPA 实体对应的 Roo 生成的 FlightDescription.as ActionScript 类：

```
FlightDescription.as
package sample.roo.flightapp.domain{
[RemoteClass(alias="sample.roo.flightapp.domain.FlightDescription")]
public class FlightDescription {
public var destination:String;
public var id:Number;
public var origin:String;
public var price:Number;
public var version:Number;
}
}
```

此代码显示，FlightDescription.as ActionScript 类定义的属性与相应的 FlightDescription JPA 实体相同。[RemoteClass] 元数据标签指定 ActionScript 对象映射到的远程 Java 对象。alias 属性指定 ActionScript 对象映射到的远程 Java 对象的完全限定类名。

Roo 为每个 JPA 实体创建 ActionScript 和 MXML 文件，以便可以从脚手架 Flex 用户界面对 JPA 实体执行 CRUD 操作。下表描述了这些 Roo 生成的文件（位于 ROOT\src\main\flex\sample\roo\flightapp\presentation\flightdescription 和 ROOT\src\main\flex\sample\roo\flightapp\presentation\flight 目录中）：

| 文件 | 描述 |
| :--- | :--- |
| `<JPA-entity-name>Event.as` | `flash.events.Event` 的子类，用于定义不同的事件类型，例如当 JPA 实体被创建、编辑或修改时生成的创建、编辑和删除事件。在 `flightapp_flex` 项目中，`FlightEvent.as` 和 `FlightDescriptionEvent.as` 表示事件类。 |
| `<JPA-entity-name>View.mxml` | 显示实体实例列表以及创建、编辑和删除实体实例选项的 MXML 文件。在 `flightapp_flex` 项目中，`FlightView.mxml` 和 `FlightDescriptionView.mxml` MXML 文件分别显示 Flight 和 FlightDescription 实体实例列表，以及创建、编辑和删除实体实例的选项。 |
| `<JPA-entity-name>Form.mxml` | 显示创建实体实例表单的 MXML 文件。在 `flightapp_flex` 项目中，`FlightForm.mxml` 和 `FlightDescriptionForm.mxml` 文件分别显示创建 Flight 和 FlightDescription JPA 实体实例的表单。 |

以下代码显示了 Roo 创建的 `FlightDescriptionEvent.as` ActionScript 类：

```
package sample.roo.flightapp.presentation.flightdescription
{
    import flash.events.Event;
    import sample.roo.flightapp.domain.FlightDescription;
    public class FlightDescriptionEvent extends Event {
    public static const CREATE:String =
    "flightDescriptionCreate";
    public static const UPDATE:String =
    "flightDescriptionUpdate";
    public static const DELETE:String =
    "flightDescriptionDelete";
    public var flightDescription:FlightDescription;
    public function FlightDescriptionEvent(type:String,
    flightDescription:FlightDescription,
    bubbles:Boolean = true, cancelable:Boolean = false) {
    this.flightDescription = flightDescription;
    super(type, bubbles, cancelable);
    }
    }
}
```

FlightDescriptionEvent 类是 flash.events.Event 类的子类，并定义了三种不同的事件类型：flightDescriptionCreate、flightDescriptionUpdate 和 flightDescriptionDelete。FlightDescription ActionScript 对象（对应于服务器端的 FlightDescription JPA 实体）表示 FlightDescriptionEvent 事件类型所携带的有效载荷。

如前所述，在 flightapp_flex 应用程序中，flightapp_flex_scaffold.mxml 文件定义了应用程序的初始用户界面。执行 flex remoting all 命令时，我们在输出中看到了 flightapp_flex_scaffold.mxml 文件已更新。以下代码显示了 Roo 对 flightapp_flex_scaffold.mxml 文件所做的修改：

```
flightapp_flex_scaffold.mxml
...
<fx:Declarations>
  <s:ArrayList id="entities">
    <fx:String>FlightDescription</fx:String>
    <fx:String>Flight</fx:String>
  </s:ArrayList>
</fx:Declarations>
...
</fx:Declarations>
...
<s:Panel id="entityPanel" title="Entity List" height="100%">
  <s:List id="entityList" dataProvider="{entities}"
    width="100%" height="100%"
    toolTip="Double-Click the selected Entity"
    doubleClickEnabled="true"
    doubleClick="entityList_doubleClickHandler(event)" />
</s:Panel>
</s:Group>
```

如果将此代码与我们在之前教程中看到的 flightapp_flex_scaffold.mxml 文件代码进行比较，您会发现唯一的变化是向 <ArrayList> 添加了 <fx:String> 元素。Roo 为应用程序中的每个 JPA 实体创建一个 <fx:String> 元素。默认情况下，<fx:String> 元素的值是相应 JPA 实体的简单名称。由于 <List> 组件使用 <ArrayList> 作为其数据提供程序，<List> 组件现在在用户界面中显示 Flight 和 FlightDescription 列表项，如下所示：

| 实体列表 | |
| --- | |
| FlightDescription | |
| Flight | |

此屏幕截图显示，Roo 并未为 FlightDescription JPA 实体中定义的查找器方法 findFlightDescriptionsByDestinationAndOrigin 生成对应的列表项。

当您双击上面列表中的某一项时，将调用 `flightapp_flex_scaffold.mxml` 文件中定义的 `entityList_doubleClickHandler` ActionScript 方法，该方法会显示由 `FlightDescriptionView.mxml` 或 `FlightView.mxml` 生成的用户界面，具体取决于双击的列表项。以下代码显示了 `entityList_doubleClickHandler` 方法：

```
protected function
entityList_doubleClickHandler(event:MouseEvent):void {
..
var selectedEntity:String = entityList.selectedItem;
var selectedEntityPackage:String =
selectedEntity.toLowerCase();

var viewClass:Class =
getDefinitionByName("sample.roo.flightapp.presentation."
+ selectedEntityPackage+"::"+selectedEntity+"View")
as Class;
if (viewClass != null) {
var newView:UIComponent = UIComponent(new viewClass());
...
mainGroup.addElement(newView);
}
...
}
```

由于 MXML 文件被编译为 ActionScript 类，`FlightDescriptionView.mxml` 和 `FlightView.mxml` 文件分别转换为 `FlightDescriptionView` 和 `FlightView` ActionScript 类。`entityList_doubleClickHandler` 方法从列表中获取所选项的值（即 **航班描述** 或 **航班**），并向其追加 'View' 字符串——使连接后的值变为 `FlightDescriptionView` 或 `FlightView`。然后，`entityList_doubleClickHandler` 创建 `FlightDescriptionView` 或 `FlightView` 的实例，并将其添加到主用户界面中。

以下序列图总结了 entityList_doubleClickHandler 方法所起的作用：

![](img/b8c379a2ed921ead3aa2b01811dc29fa_282_0.png)

需要注意的是，flightapp_flex_scaffold.mxml 文件的 entityList_doubleClickHandler 方法从不直接引用 FlightView 或 FlightDescriptionView ActionScript 类。实际上，flightapp_flex 项目中的任何其他 MXML 或 ActionScript 类都不引用 FlightView 和 FlightDescriptionView 类。这的副作用是，Flex 编译器不会将 FlightDescriptionView 和 FlightView 包含在生成的 SWF 文件中。为了指示 Flex 编译器包含 FlightDescriptionView 和 FlightView ActionScript 类，Roo 将它们的完全限定名称添加到 flightapp_flex_scaffold-config.xml 文件中，如下所示：

```
flightapp_flex_scaffold-config.xml
<flex-config xmlns="http://www.adobe.com/2006/flex-config">
<includes append="true">
<symbol>sample.roo.flightapp.presentation.flightdescription.FlightDescriptionView</symbol>
<symbol>
sample.roo.flightapp.presentation.flight.FlightView
</symbol>
</includes>
</flex-config>
```

<symbol> 元素指定了 Flex 编译器应在生成的 SWF 文件中包含的 ActionScript 类。
