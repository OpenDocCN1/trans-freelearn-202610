

```xml
android:layout_height="wrap_content"
android:textAppearance="?android:attr/textAppearanceLarge"
android:text="Large Text"
android:id="@+id/txtName"
android:layout_alignParentTop="true"
android:layout_alignParentLeft="true"
android:layout_alignParentStart="true" >
```

```xml
<TextView
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:textAppearance="?android:attr/textAppearanceLarge"
    android:text="Large Text"
    android:id="@+id/txtAddress1"
    android:layout_below="@+id/txtName"
    android:layout_alignParentLeft="true"
    android:layout_alignParentStart="true" >
```

```xml
<TextView
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:textAppearance="?android:attr/textAppearanceLarge"
    android:text="Large Text"
    android:id="@+id/txtAddress2"
    android:layout_below="@+id/txtAddress1"
    android:layout_alignParentLeft="true"
    android:layout_alignParentStart="true" >
```

```xml
<TextView
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:textAppearance="?android:attr/textAppearanceLarge"
    android:text="Large Text"
    android:id="@+id/txtZip"
    android:layout_below="@+id/txtAddress2"
    android:layout_alignParentLeft="true"
    android:layout_alignParentStart="true" >
</RelativeLayout>
```

现在我们所要做的就是添加一个托管 Activity，以便在纵向模式下查看详情 Fragment 时使用。

## 为 PortraitDetailActivity 编写代码及其布局

首先，我们需要一个非常简单的布局。要创建简单的布局，请执行以下步骤：

- 1. 创建名为 activity_portrait_layout 的布局。
- 2. 添加一个 FrameLayout。
- 3. 将其 id 属性设置为 detailFragmentHolder。

现在，我们可以创建一个名为 PortraitDetailActivity 的新 Activity。方法是右键单击包含所有 Java 文件的文件夹。导航到 New | Activity | Blank Activity。这几乎与创建新类并添加 extends... 代码以将其转换为 Activity 相同。以这种方式操作的区别在于，Android Studio 还会为我们将新 Activity 添加到 AndroidManifest.xml 文件中，从而节省了几分钟的工作。

在 PortraitDetailActivity 的 onCreate 方法中（这是本类中我们需要关心的唯一方法），我们执行以下操作：

- 1. 首先，我们获取 FragmentManager 的引用并开始一个事务。
- 2. 其次，我们获取现有 Fragment 的引用并创建一个新引用。现在，我们检查旧 Fragment 是否实际存在，如果存在，则将其移除/销毁。
- 3. 然后，我们提取 MainActivity 传入的单个 int 变量。最后，以常规方式添加新 Fragment，传入要显示的 NameAndAddress 对象的位置。
- 4. 添加下面所示的刚刚讨论过的重写的 onCreate 方法：

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_portrait_detail);

    // Get a fragment manager
    FragmentManager fManager = getFragmentManager();

    // Create a new fragment using the manager
    // Passing in the id of the layout to hold it
    Fragment frag =
    fManager.findFragmentById(R.id.detailFragmentHolder);

    // Pass the Bundle onto the fragment
    int position = 0;
    Bundle extras = getIntent().getExtras();
    if (extras != null) {
    position = extras.getInt("Position");
    }

    // Check the fragment has not already been initialized
    if(frag == null){
        frag = AddressDetailFragment.newInstance(position);

        fManager.beginTransaction()
        .add(R.id.detailFragmentHolder, frag)
        .commit();
    }
}
```

现在我们可以以纵向模式运行应用并查看列表：

## 双 Fragment

- B 奥巴马
- E 温莎
- V 普京

点按某个项目，查看详细信息在新 Activity 中打开：

![](img/35d01831c19688a44e50bce7c489007a_661_0.png)

点按返回按钮，将设备旋转为横向模式，并点按 **V 普京** 以查看我们的两个 Fragment 并排显示：

![](img/35d01831c19688a44e50bce7c489007a_662_0.png)

让我们通过查看一些常见问题来思考何时应该使用 Fragments，何时不应该使用。

## 常见问题

- 1. 我何时应该使用 Fragments？

希望，你在本章中已经学到了足够多的内容，认识到使用 Fragments 可以进一步增强我们代码的可重用性和封装性，并且每当我们实现的内容不仅仅是一个简单的 UI 时，使用 Fragments 所带来的开销也是值得的。

- 2. 还有哪些时候我应该使用 Fragments？

还要考虑到，即使是最简单的应用也可能会发展。如果你简单的仅限手机的单屏应用突然需要移植到平板电脑，你将需要重构所有内容以使用 Fragments。这总是比从一开始就使用 Fragments 更困难。

- 3. 所以你是说我应该始终使用 Fragments 吗？

可能是的！除非你有极充分的理由不使用。然而，在这本书中，为了学习新概念（例如在 Android 设备上捕获图像或在地图中显示用户位置），我们将不让 Fragments 妨碍新代码。

## 总结

这可能是我们构建过的最复杂的应用之一。如果对确切发生的事情仍有不清楚之处，克服这一点的方法是将其分解为片段（或 fragments）。

每个 Fragment 都有一个类和一个布局。带有列表的 Fragment 通过接口与 `Activity` 通信，Activity 要么将一个新的（详情）Fragment 加载到自身中（在横向模式下），要么启动一个新的 `Activity`，该 Activity 在纵向模式下加载相同的（详情）`Fragment`。所有数据都存储在我们的单例中，基本上可以忽略不计，因为它只能实例化一次，并且保证任何更改或读取数据的类都将从同一个实例进行操作。

当然，本章中的某些代码方面或原则对你来说可能仍然不清楚，但通过反复使用，你可以使它们变得像本能一样自然。

在下一章中，我们不会再增加复杂度。所以，如果所有这些 Fragments 和接口都不完全清楚，也没关系。我们将在本书剩余的内容中逐一重新讨论这些概念，同时深入了解 Android API。

如果你曾经想制作像愤怒的小鸟关卡选择菜单那样酷炫的滑动 UI（可以在屏幕之间滑动），那么下一章就是为你准备的。而且正如你可能料到的，在过程中我们会更多地熟悉 Fragments。

## 第 20 章 分页和滑动

分页是指从一页移动到另一页，在 Android 上，我们通过在屏幕上滑动手指来实现这一点。当前页面会以与手指移动相匹配的方向和速度进行过渡。这是一种在应用中导航的有用且实用的方式，但也许更重要的是，它为用户提供了极其令人满意的视觉效果。此外，与 ListView 类似，我们可以选择性地仅加载当前页所需的数据，也许还包括前一页和下一页的数据。

在下一个截图中，你可以看到 Nexus 5 模拟器上的日历应用在月份之间分页。如果你不确定分页和滑动的工作原理，请务必启动模拟器并亲自试用：

![](img/35d01831c19688a44e50bce7c489007a_666_0.png)

在下一个图中，我们可以看到著名的愤怒的小鸟关卡选择菜单展示滑动/分页的实际效果：

### “页面”滚入和滚出视图

![](img/35d01831c19688a44e50bce7c489007a_667_0.png)

正如你现在所料，Android API 提供了一些解决方案，可以让你以相当简单的方式实现分页。

在本章中，你将学习如何：

- 使用图片实现分页和滑动，就像在照片库应用中所做的那样。
- 使用 Fragments 实现分页和滑动，这将使我们的应用有可能让用户能够在多个用户界面之间滑动。这将为我们的应用带来巨大的潜力。

我们不会将分页/滑动添加到 Where it's snap 应用中，但让我们构建两个分页小应用：一个使用图片，另一个使用 Fragments。

