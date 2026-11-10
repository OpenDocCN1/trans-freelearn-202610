
## 创建规则文件

![](img/b18244e6621ecc39cef03ff6c0e55e7c_215_0.png)

图 14.7：规则文件编辑器

建议在创建过程中通过选择 File | Save 保存规则文件。在任何时候都可以关闭规则编辑器并退出，选择选项 File | Close。

5. 参考图 14.8；在 Tree View 选项卡下从模板文件选择一个 XML 元素（例如 <BANNERIMG>）并选择菜单选项 Rules | Add Rule。

在 Web Publisher 编辑器中可以为模板 XML 元素创建以下类型的部件：

- **文本行**：这是一个单行文本字段部件，允许内容作者在没有任何格式的情况下输入文本
- **内容**：这是一个多行文本字段部件，允许内容作者输入文本并通过可用的菜单选项对其进行格式化，例如：加粗、斜体、更改字体等。
- **选择**：这是一个下拉选择列表，允许内容作者从现有值中选择特定值。列表中的值可以从硬编码的固定列表中填充，也可以通过 Docbase 中的 DQL 查询动态填充。
- **文本选择器**：这是一个部件，允许内容作者从 Docbase 中选定的现有文件中选择一些文本。
- **图形**：这是一个选择列表部件，允许内容作者从 Docbase 中现有的图像中选择图像。
- **复选框：** 这是一个复选框部件，允许内容作者选择或取消选择它。
- **X 选择器：** 这是一个文件选择部件，允许内容作者从 Docbase 中现有文件列表中选择文件。
- **重复块：** 这是一个用作其他元素容器的部件。将一组元素/字段指定为可重复允许您在编辑器中多次呈现这些元素。这允许内容作者复制特定字段或为同一字段的多个实例指定多个值。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_216_0.png)

图 14.8：添加 Graphic 规则

如图所示 14.8，选择 Graphic 选项为 <BANNERIMG> 模板元素添加规则。

6. 填写图 14.9 所示的图形部件的字段。为字段提供一些标签，例如 Banner Image。为内容作者的方便提供一些说明性标签，例如 Please choose a banner image。通过选择 Required 旁边的复选框使字段成为必填项。

7. 定义用于显示 Docbase 中现有图像的选择列表，内容作者可以从中进行选择。将显示选项选择为 List control。
输入 DQL 查询以填充选择列表：

```
dm_document where folder(/Test_WebCabinet/Custom_Images)
```

请注意，您无需提供关键字 select * from。

这里的假设是，在 Test_WebCabinet Web 柜内的 Docbase 中预先存在一个包含图像文件的 Custom_Images 文件夹。如果不存在，请创建它并在其中放置一些图像。

要在 select 查询中使用的属性名称需要提供，例如 object_name。这最终转换为以下 DQL：

```
select object_name from dm_document where
folder(/Test_WebCabinet/Custom_Images)
```

单击 Test Query 按钮以检查输入的 DQL 语法是否正确，并找出在运行时将在部件中显示的查询结果。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_217_0.png)

图 14.9：为图形元素定义规则

8. 您可以通过选择 Allow import 复选框，可选地提供内容作者通过图形部件在 Docbase 中指定文件夹内导入图像的选项。
9. 单击图 14.10 中显示的 Browse Docbase 按钮并选择图像文件将被导入并保存到的文件夹。
10. 通过单击 Type 字段旁边的 Select 按钮，选择导入的图像将与其关联的对象类型。

11. 通过单击 Lifecycle 字段旁边的 Select 按钮，选择导入的图像将附加到的生命周期。

12. 通过单击 Category 字段旁边的 Select 按钮，选择导入的图像将与其关联的类别。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_218_0.png)

图 14.10：在图形元素中允许图像导入选项

13. 保存规则文件并单击 Preview 选项卡（参考图 14.11），以查看当内容作者使用 News Article 模板创建内容时横幅图像字段的外观。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_218_1.png)

图 14.11：横幅图像图形部件的预览

请注意，Import 按钮也已添加，允许您从本地计算机导入图像文件到 Docbase 内的指定文件夹位置。

14. 为了查看图形元素的自动系统生成的规则文件代码，请单击 Rule XML 选项卡（参考图 14.12）：

```
<tagcontent tag_name="BANNERIMG">
<graphic enable_locale_fallback="N"
    import="Y"
    instruction="Please choose a banner image"
    label="Banner Image:"
    lifecycle="Default Lifecycle"
    location="/Test_WebCabinet/Custom_Images"
    output_property="folderPath"
    property="object_name"
    query="dm_document where folder(&#39;/Test_WebCabinet/Custom_Images&#39;)"
    query_type="query"
    required="Y"
    type="dm_document">
</graphic>
</tagcontent>
```

图 14.12：系统生成的规则文件代码

同样，您可以通过选择图 14.13 中显示的规则编辑器部件类型为剩余的模板字段创建其他部件。

| 模板 XML 元素 | 规则文件部件 |
|---|---|
| BANNERIMG | Graphic |
| IMGALTTEXT | Textline |
| DATE | Textline |
| BODYTEXT | Content |
| REPEATBLOCK | Repeating Block |
| NEXTLINKNAME | Textline |
| NEXTLINKURL | Textline |

图 14.13：规则文件部件

15. 在突出显示相应的模板 XML 元素并为其添加规则后，您可以保存规则文件并通过选择菜单选项 File | Close 退出编辑器。

检入新创建的规则文件（cust_news_art_rule.xml）并通过单击 Make Available 按钮使模板文件（cust_news_art_temp1.xml）可用，如上一章所述。

为方便起见，下面显示了一些在为剩余模板 XML 元素创建规则时拍摄的屏幕截图。您可以在创建规则文件时将其作为指南：

### 14.1.1 IMGALTTEXT 元素

![](img/b18244e6621ecc39cef03ff6c0e55e7c_220_0.png)

图 14.14：IMGALTTEXT 元素的 Textline 部件

### 14.1.2 DATE 元素

![](img/b18244e6621ecc39cef03ff6c0e55e7c_220_1.png)

图 14.15：DATE 元素的 Textline 部件

请注意，图 14.15 中的默认日期格式已设置为：TODAY+0,MMM dd, yyyy。

每当使用 News Article 模板创建内容时，News date: 字段将以指定格式预先填充当天的日期。

### 14.1.3 BODYTEXT 元素

![](img/b18244e6621ecc39cef03ff6c0e55e7c_221_0.png)

图 14.16：BODYTEXT 元素的 Content 部件

在图 14.16 中，可以为多行文本字段选择众多格式选项。此外，您可以选择 Links 选项旁边的复选框，以允许内容作者创建指向 Docbase 中现有文件的超链接。Link browser formats 允许您指定希望为内容作者选择显示的各种文件格式。默认情况下，仅启用 html 和 crtext 格式，允许显示具有 .htm、.html 和 .txt 扩展名的文件。

### 14.1.4 REPEATBLOCK 元素

![](img/b18244e6621ecc39cef03ff6c0e55e7c_221_1.png)

图 14.17：为 REPEATBLOCK 元素设置重复块

<REPEATBLOCK> 模板元素包含 <NEXTLINKNAME> 和 <NEXTLINKURL> 元素，表示内容作者可以在此块内提供多个文章链接名称及其 URL。为了使 <REPEATBLOCK> 成为重复块部件，请突出显示它并选择菜单选项 Rules | Set Repeating Block，如图 14.17 所示。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_222_0.png)

图 14.18：定义重复块

为方便内容作者，在重复块中提供标签和说明性文本，如图 14.18 所示。

### 14.1.5 NEXTLINKNAME 元素

![](img/b18244e6621ecc39cef03ff6c0e55e7c_222_1.png)

图 14.19：NEXTLINKNAME 元素的 Textline 部件
