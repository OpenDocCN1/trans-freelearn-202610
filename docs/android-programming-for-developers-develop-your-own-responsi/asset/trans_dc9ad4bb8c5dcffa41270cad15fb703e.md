

```java
public View getView(int whichItem, View view, ViewGroup viewGroup) {
    if (view == null) {
        LayoutInflater inflater = (LayoutInflater)
        getActivity().getLayoutInflater();
        view = inflater.inflate(R.layout.list_item, null);
    }
    // We also have this super-handy getItem method
    NameAndAddress tempNameAndAddress = getItem(whichItem);
    TextView txtName = (TextView)
    view.findViewById(R.id.txtName);
    txtName.setText(tempNameAndAddress.getName());
    return view;
}
}
```

现在，我们需要为列表项创建一个名为 list_item.xml 的布局。以通常的方式在 layout 文件夹中创建一个名为 list_item 的新布局资源文件，将一个 Plain TextView 小部件拖放到布局上，并将 id 属性设置为 txtName。

在处理布局时，我们需要修改 activity_main 和 activity_main_land 布局文件。前者应包含一个 Fragment，后者应包含并排排列的两个 Fragment。我们将设置用于放置 Fragment 的 ID。

在 activity_main.xml 中，删除竖屏 TextView 小部件，并从调色板的 Containers 类别中添加 FrameLayout。将其 id 属性设置为 listFragmentHolder。以下是 activity_main.xml 中你现在应该拥有的代码。检查你是否拥有与我们刚才讨论的以下代码相同的代码：

```xml
<RelativeLayout
xmlns:android="http://schemas.android.com/apk/res/android"
xmlns:tools="http://schemas.android.com/tools"
android:layout_width="match_parent"
android:layout_height="match_parent"
android:paddingLeft="@dimen/activity_horizontal_margin"
android:paddingRight="@dimen/activity_horizontal_margin"
android:paddingTop="@dimen/activity_vertical_margin"
android:paddingBottom="@dimen/activity_vertical_margin"
tools:context=".MainActivity"
android:id="@+id/layout">
<FrameLayout
android:layout_width="match_parent"
android:layout_height="match_parent"
android:layout_alignParentTop="true"
android:layout_alignParentLeft="true"
android:layout_alignParentStart="true"
android:id="@+id/listFragmentHolder"/></FrameLayout>
</RelativeLayout>
```

切换到 activity_main_land.xml 文件并删除横屏 TextView 小部件。

将现有的 LinearLayout 更改为将 orientation 属性设置为 horizontal。将两个 FrameLayout 拖放到布局上，并将它们的 width 属性都设置为 unset，layout_weight 属性都设置为 .5。将左侧 FrameLayout 的 id 属性设置为 listFragmentHolder，将右侧 FrameLayout 的 id 属性设置为 detailFragmentHolder。

为确保 activity_main_land.xml 的代码完全正确，你可以将其与此清单对比。如果需要，直接复制并粘贴代码：

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
xmlns:android="http://schemas.android.com/apk/res/android"
android:orientation="horizontal"
android:layout_width="match_parent"
android:layout_height="match_parent">
<FrameLayout
android:layout_width="wrap_content"
android:layout_height="match_parent"
android:id="@+id/listFragmentHolder"
android:layout_weight=".5"/>
<FrameLayout
android:layout_width="wrap_content"
android:layout_height="match_parent"
android:id="@+id/detailFragmentHolder"
android:layout_weight=".5"/>
</LinearLayout>
```

activity_main_land 布局现在应如设计器中下一个屏幕截图所示。请记住，你可以在设计器中切换横屏和竖屏视图，但无论以何种方式在设计器中查看布局，它在应用中都能正常工作。设计器不知道我们的 Java 代码或别名将会做什么：

![](img/35d01831c19688a44e50bce7c489007a_645_0.png)

进展顺利。此时，我们可以运行应用，来自通讯录的略显简短且有些不太可能的姓名列表将在竖屏和横屏模式下都显示出来。

横屏视图可能不是我们期望的那样，因为列表侧会扩展以填满整个屏幕，而详情侧目前为空。但我们的两个不同布局都使用相同的 AddressListFragment 类，它本身不需要布局文件，因为 ListView 由 ListFragment 提供。

现在，我们需要管理列表项被点击时的情况。我们已经知道如何处理列表项点击，但有一些小问题：

- 如果在横屏视图中点击列表项，我们需要在详情窗格（右侧）中显示相应的详情
- 如果在竖屏视图中点击列表项，我们需要一个全新的 Activity 来显示详情（尽管仍然是同一个 Fragment 的一个实例）

无论是哪种情况，详情视图都是一个不同的类。因此，我们需要以某种方式与该类进行通信。在研究解决方案之前（我们之前在本章前面的 *跨 Fragment 通信 – 重新审视接口* 部分已经讨论过），值得注意的是，我们尚未创建这个类。

然而，最好先处理通信问题。我们需要一种高效的方式让 AddressListFragment 与 MainActivity 通信，以便 MainActivity 可以填充详情窗格（如果是横屏模式）或启动一个新 Activity 来显示详情窗格（如果是竖屏模式）。

我们将首先编写通信问题的解决方案。然后，我们将创建 AddressDetailFragment，它可以同时用于我们的双 Fragment 横屏布局以及一个名为 PortraitDetailActivity 的新 Activity，该 Activity 用于以竖屏方式持握的设备。

## 创建通信接口

这段代码将出乎意料地简单，并且与我们在本章前面的 *跨 Fragment 通信 – 重新审视接口* 部分讨论的几乎完全相同。唯一的区别是，我们将看到确切的接口实现位置和方式，并且 onListItemClicked 方法将被完整编码，以查看它如何要么启动一个包含 Fragment 详情的新 Activity，要么在横屏模式下重新填充/刷新现有的 Fragment 详情。

我会先展示代码，然后我们将再次讨论在哪里以及如何使用代码，以巩固我们正在做的事情：

```java
public interface ActivityComs {
    void onListItemSelected(int pos);
}
```

我们将创建一个（刚才看到的）带有单个方法的接口。然后我们将让 MainActivity 实现这个接口，这样它就被迫重写 onListItemSelected 方法。

然后，AddressListFragment 类可以在点击列表项时调用该方法，并传递适当的 NameAndAddress 引用及其在 ArrayList 中的位置。我们将看到为什么要同时使用这两个数据。

MainActivity 然后可以处理要么启动一个新 Activity（当处于竖屏模式时），要么直接将数据传递给 DetailFragment（当处于横屏模式时）。

右键单击包含所有 .java 文件的文件夹，导航到 新建 | 类。在名称字段中输入 ActivityComs，并在名为 Kind 的下拉列表中选择 接口。然后单击 确定，并按照之前代码块所示准确编写新接口。

接下来，我们需要在 AddressListFragment 类中创建此接口的一个实例作为成员。在类声明后立即添加此成员，如下一个代码片段中突出显示的那样：

```java
public class AddressListFragment extends ListFragment {
private ActivityComs mActivityComs;

private ArrayList<NameAndAddress> mNamesAndAddresses;
```

现在，我们需要将成员初始化为指向 MainActivity 但转换为 ActivityComs 类型的引用，我们在重写的 onAttach 方法中执行此操作。然后，我们需要在 onListItemClick 方法中使用它，最后，我们需要通过在 onDetach 方法中将其设置为 null 来删除引用。将这三个方法添加到 AddressListFragment 类中，如下一个代码片段所示：

```java
@Override
public void onAttach(Activity activity) {
    super.onAttach(activity);

    /*
    activity is a ActivityComs but must
    still be explicitly cast to the type
    */
    mActivityComs = (ActivityComs)activity;
}
```

```java
@Override
public void onDetach() {
    super.onDetach();
    mActivityComs = null;
}
```

```java
public void onListItemClick(ListView l, View v, int position, long id) {
    // pass the position to MainActivity
    mActivityComs.onListItemSelected(position);
}
```

现在，更改 MainActivity.java 中的类声明以实现我们的新接口：

```java
public class MainActivity extends ActionBarActivity implements ActivityComs{
```

现在，我们需要在 ... 中实现 onListItemClicked 方法

