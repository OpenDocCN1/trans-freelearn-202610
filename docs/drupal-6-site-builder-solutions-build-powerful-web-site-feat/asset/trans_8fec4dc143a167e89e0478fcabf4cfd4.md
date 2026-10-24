

4. 通过允许第三方站点单独维护内容，您还可以利用他们对其站点技术所做的任何改进。我们描述的链接站点都非常庞大且积极维护。它们还倾向于频繁发布新功能，而自行实现这些功能可能困难且昂贵。

## 在线烹饪课程

在本节中，我们将添加来自 YouTube 的视频内容，演示餐厅中使用的各种烹饪技巧。

### 使用过滤器嵌入 YouTube 视频

*Good Eatin' Goal:* 将存储在 YouTube 上的视频添加到现有页面。

*Additional modules needed:* Video Filter (http://drupal.org/project/video_filter)。

### 基本步骤

YouTube (www.youtube.com) 是一个流行的视频分享网站，您可以在此观看全球各地人们创建和提交的视频。YouTube 允许您链接到存储在其网站上的视频，以便与自己的访客分享，而无需承担带宽费用。事实上，如果您拥有 Google AdSense 账户，您甚至可以从视频中获得收入。

要将视频插入页面，我们将使用以下流程：

- 1. 安装并激活 **Video Filter** 模块。
- 2. 我们现在需要构建一个新的输入格式来处理该过滤器。为此，请从 **Administer** 菜单中选择 **Site configuration**，然后选择 **Input formats**。
- 3. 点击 **Add input format** 链接。
- 4. 我们将输入格式命名为 YouTube，并仅允许管理员使用此格式。根据您站点的需求，您需要决定谁可以使用输入格式。只需确保任何有权访问此格式的人都将遵守您站点的条款，并且不会链接到您不希望出现在站点上的视频。
- 5. 最后，启用 **Video Filter** 过滤器并 **Save the configuration**。

![](img/b332d58f89551dfc3ae9a7cf47e6a08f_216_0.png)

6. 我们现在可以在新页面中使用新的输入格式。我们将通过点击 **Create content** 然后点击 **Page** 来创建页面。

7. 我们将新页面的标题设为 **Cooking Classes**，并按如下方式输入描述：

![](img/b332d58f89551dfc3ae9a7cf47e6a08f_216_1.png)

保存页面后，最终结果将如下所示：

## Cooking Classes

Page Cooking Classes has been created.

Chinese dumplings are a popular appetizer on the Good Eatin' menu. We make ours with a variety of rare meats including ostrich, emu, and crocodile rather than more traditional chicken and pork.

![](img/b332d58f89551dfc3ae9a7cf47e6a08f_217_0.png)

您还可以使用 Video Filter 模块通过使用相同的技术来显示来自 Google Video、GodTube、DailyMotion、Eyespot、Jumpcut、Revver 和 Vimeo 的视频。要使用这些替代来源，只需在过滤器中输入要嵌入的页面的完整 URL 即可。

在将 YouTube 视频添加到您的站点之前，请确保您遵守 YouTube 的使用条款，并检查该材料是否未受版权保护。有关更多详细信息，请参阅 http://www.youtube.com/t/terms。

## 创建 YouTube 播放列表并显示随机视频

Good Eatin' Goal: 增强 YouTube 过滤器以从视频列表中播放，并探索 Video Filter 模块的其他功能。

Additional modules needed: Video Filter (http://drupal.org/project/video_filter)。

### 基本步骤

现在您可以在页面中添加单个视频，您可能希望添加更多以随机顺序显示的视频，自动在页面加载时开始视频，或控制视频大小。所有这些都可以通过使用 Video Filter 实现。

要向过滤器添加多个视频，您只需添加多个视频并用逗号分隔。例如，您可以输入：

```
video:http://www.youtube.com/watch?v=0Q2aPi9ZEgs,
http://www.youtube.com/watch?v=dTDVKDzVOcg,http://www.youtube.com/watch?v=eV__oOckAPM
```

如果您希望视频自动开始，您需要在过滤器中添加 autoplay:1，并与过滤器的其余部分用空格分隔。例如：

```
video:http://www.youtube.com/watch?v=DqoqFymiGkk autoplay:1
```

要调整视频大小，您需要输入视频的宽度和高度，再次与过滤器的其余部分用空格分隔。过滤器将确保视频的纵横比正确，并且实际宽度和高度不超过您指定的大小。下面是一个示例：

```
video:http://www.youtube.com/watch?v=BYhrHdvI6m8 width:240
height:160
```

最后的格式选项是控制页面上视频的对齐方式。请注意，您目前只能指定对齐方式为左或右。这是通过 align 选项控制的，如下所示：

```
video:http://www.youtube.com/watch?v=wPhBiu6Sx_w align:right
```

## 使用嵌入式媒体字段增强内容

Good Eatin' Goal: 将 YouTube 视频集成到现有内容类型中，而不是使用过滤器。

Additional modules needed: Embedded Media Field (http://drupal.org/project/emfield)。

### 基本步骤

在最后两个主题中，我们探讨了将 YouTube 视频插入现有内容的方法。但如果您想为特定内容类型的所有实例添加视频字段怎么办？例如，如果您有一个烹饪课程的内容类型，您可以包含以下字段：

- Class Name
- Class Description
- Class Dates
- Video giving a preview of the class

在这种情况下，最好为视频设置一个特定字段，而不是使用过滤器将其嵌入文本中。

让我们使用 Embedded Media 模块来添加视频字段来构建上面的示例。

- 1. 首先在模块管理器中安装并启用 **Embedded Media Field** 和 **Embedded Video Field** 模块。
- 2. 我们现在可以通过从 **Administer** 菜单中选择 **Content Management** 然后选择 **Content types** 来创建新的内容类型。最后，点击 **Add content type**。
- 3. 我们将按如下方式填写内容类型的基本信息：

![](img/b332d58f89551dfc3ae9a7cf47e6a08f_219_0.png)

点击 Save content type 保存类型。

9. 可能的提供商完整列表如下所示。我们将仅为我们的字段选择 YouTube。

### Providers Supported

从下面的列表中选择您希望允许此内容类型的第三方提供商。如果未选中任何复选框，则将支持所有提供商。当用户提交新内容时，他们输入的 URL 将与提供商匹配，前提是该提供商在此处被允许。

Providers:

- Blip.tv
- Brightcove
- Dailymotion
- Google
- GUBA
- IMEEM
- JumpCut
- Last.fm
- Live Video
- MetaCafe
- MySpace
- Revver
- Sevenload
- Spike TV
- Tudou
- Veoh
- Vimeo
- YouTube
- Custom URL

10. 下一组设置是显示和预览设置，每个设置都包含以下屏幕截图中显示的相同选项集：

### Video Display Settings

这些设置控制此视频以其全尺寸显示的方式，默认值为 425x350。

Video display width: *
425
视频的宽度。默认为 425。

Video display height: *
350
视频的高度。默认为 350。

Autoplay
如果提供商支持，选中此框将导致视频在全尺寸时加载后自动开始。

您可以使用预览设置在某些情况下显示较小的视频。我们将保留所有其他设置使用其默认值。

11. 下一组设置控制预览图像，如以下屏幕截图所示：

![](img/b332d58f89551dfc3ae9a7cf47e6a08f_222_0.png)

12. 最后，全局设置可以按如下所示设置。我们将把该字段标记为 Required。

![](img/b332d58f89551dfc3ae9a7cf47e6a08f_222_1.png)

13. 点击 Save field settings 按钮以完成添加字段。

