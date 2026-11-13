

## 此书中使用的权限方案

在着手编写本书之前，我们非常仔细地考虑过，在本书项目中使用 Marshmallow 运行时权限所需的覆盖量会掩盖其他学习目标，并对开发者读者或我们未来的用户提供很少的即时帮助。尤其是在本书需要出版新版之前，Marshmallow 设备的普及率不太可能达到 10% 的市场份额。除此之外，有一个非常简单的变通方法可以让我们的所有应用与 Marshmallow 和 Marshmallow 之前的权限兼容，我们将在第 21 章“Navigation Drawer and Where It's Snap”中构建带有危险权限的首个应用时看到这一点。

现在我们已经考虑了权限，让我们构建需要 android.permission.INTERNET 权限的 Widget 演示应用。这被视为普通权限，因此不需要针对 Marshmallow 设备进行任何特殊处理。

## Widget 探索小应用

我们刚才讨论了七个小部件。EditText、ImageView、RadioButton（和 RadioGroup）、Switch、CheckBox、WebView 和 TextClock 小部件。让我们做一个小应用，并对每个小部件做一些实际操作。我们还将使用 Button。

## 设置 Widget 探索项目和 UI

首先，我们将设置一个新项目并准备 UI 布局。这些快速步骤会将所有小部件放在屏幕上并设置 id 属性，以便获取对它们的引用。这将帮助我们在开始之前查看目标应用（如下图所示），该应用正在运行：

![](img/35d01831c19688a44e50bce7c489007a_382_0.png)

本应用将演示这些新小部件的方式如下：

- 单选按钮将允许用户从三个时区中选择，更改时钟上显示的时间
- 左侧的按钮在被单击时，将把其文本更改为 EditText 小部件中当前的内容
- 三个复选框将为 Android 机器人图像添加和移除视觉效果
- Switch 将打开和关闭 webView，它显示一位著名计算机科学家的相关信息

确切的布局位置并不重要，但指定的 id 属性必须完全匹配。如果你只是想查看/使用代码，可以在下载包的 Chapter 11/Widget Exploration 文件夹中找到所有文件。

- 1. 创建一个名为 Widget Exploration 的新项目，将 **最低 API** 设置为 17，使用空白 Activity，并保持其他所有设置为默认值。我们使用 API 17 是因为 **TextClock** 小部件的一个功能要求如此。
- 2. 让我们创建一个新布局文件，因为我们希望新布局基于 **LinearLayout**。在项目资源管理器中右键单击 **layout** 文件夹，并在弹出菜单的 **新建** 下选择 **布局资源文件**。
- 3. 在 **新建资源文件** 窗口中，在 **文件名** 字段输入 exploration_layout.xml，在 **根元素** 字段输入 **LinearLayout**。现在，单击 **确定**。
- 4. 在属性窗口中，将 **LinearLayout** 的方向属性更改为 **水平**。
- 5. 使用设计视图上方的下拉控件，确保在 **横向** 视图中选择了 **Nexus 5**。
- 6. 我们现在可以开始创建布局了。将三个 **RelativeLayout** 布局拖放到设计区域，以创建设计的三个垂直划分。依次将每个 **RelativeLayout** 布局的 **权重** 属性设置为 .33。我们现在有三个相等的垂直划分，就像前一幅图所示。
- 7. 将 **Switch** 和 **WebView** 从调色板拖放到 **RelativeLayout** 的右侧。我们布局的右侧现在应该看起来就像前一幅图所示。
- 8. 将三个 **CheckBox** 小部件依次上下拖放，然后将 **ImageView** 拖放到它们下方的中间 **RelativeLayout** 布局上。中间列现在应该看起来就像前一幅图所示，只是我们不能看到 **ImageView**，直到在其 src 属性中放入适当的值。我们将在几个步骤后执行此操作。
- 9. 将 **RadioGroup** 拖放到 **RelativeLayout** 的左侧，并在 **RadioGroup** 内添加三个 **RadioButton** 小部件。
- 10. 在 **RadioGroup** 下，从调色板的 **文本字段** 类别中拖放一个 **PlainText** 小部件。将 **Button** 小部件添加到 **PlainText** 小部件的右侧。从调色板添加 **TextClock** 到 **RelativeLayout** 左侧底部一半。你会看到关于 API 级别的警告。我们可以忽略它，因为我们将在最新的 Nexus 5 模拟器上运行。

![](img/35d01831c19688a44e50bce7c489007a_384_0.png)

- 11. 尝试调整布局，使其尽可能类似于前一幅图。请注意，直到我们添加实际图像之前，ImageView 是不可见的。

- 12. 现在，将下表中显示的属性添加到我们刚刚布置的小部件上：

| 小部件类型 | 属性 | 要设置的值 |
|---|---|---|
| RadioGroup | id | radioGroup |
| RadioButton (顶部) | id | radioButtonLondon |
| RadioButton (顶部) | text | London |
| RadioButton (顶部) | checked | true |
| RadioButton (中间) | id | radioButtonBeijing |
| RadioButton (中间) | text | Beijing |
| RadioButton (底部) | id | radioButtonNewYork |
| RadioButton (底部) | text | New York |
| EditText | id | editText |
| Button | id | button |
| TextClock | id | textClock |
| CheckBox (顶部) | text | Transparency |
| CheckBox (顶部) | id | checkBoxTransparency |
| CheckBox (中间) | text | Tint |
| CheckBox (中间) | id | checkBoxTint |
| CheckBox (底部) | text | Re-Size |
| CheckBox (底部) | id | checkBoxReSize |
| ImageView | src | @mipmap/ic_launcher |
| ImageView | id | imageView |
| Switch | id | switch1 |
| Switch | enabled | true |
| Switch | clickable | true |
| WebView | id | webView |

我们刚刚布置并设置了布局所需的属性。除了某些小部件类型对我们是新的之外，没有什么是我们以前没有做过的。

现在，我们可以开始在 Java 代码中使用所有这些小部件了。

## 编写 Widget 探索应用的代码

由于我们有将连接到 Internet（以显示网页）的 WebView，因此还需要用户的权限。所以，我们需要向 AndroidManifest.xml 文件添加一行。从项目资源管理器的 manifests 文件夹中打开它。

现在，在高亮显示的上下文指示的位置添加以下高亮显示的代码行：

```
...
<uses-permission android:name="android.permission.INTERNET" />
	<application
		android:allowBackup="true"
...
```

现在，让我们通过更改 onCreate 中对 setContentView 的调用，确保显示我们的新布局，使其如下所示：

```
setContentView(R.layout.exploration_layout);
```

接下来的这段代码看起来相当长且散乱，但我们所做的就是获取布局中每个小部件的引用。当我们准备使用它们时，我们将更详细地讨论代码。

接下来的代码块中唯一新的东西是某些对象被声明为 final。这是因为它们将在匿名类中使用。

### 提示

### 但是 final 不意味着对象不能被更改吗？

如果你记得，在第 9 章“面向对象编程”中，我们看到声明为 final 的变量不能被更改，它们是常量。那么，我们将如何更改这些对象的属性呢？请记住，对象是引用类型变量。也就是说，它们指向堆上的对象。它们本身并不是对象。我们可以把它们视为保存对象地址。地址是不能更改的。我们仍然可以使用该地址来引用堆上的对象，并随意更改实际对象。

