
## 编写通信接口

我们知道 TitlesFragment 需要一种与 ViewFragment 通信的方式，同时 TagsFragment 也需要一种方式来告诉 TitlesFragment 它希望使用哪个标签作为搜索条件，以查找数据库中与该标签匹配的条目。

同一个接口将处理所有这些工作。MainActivity 随后可以实现该接口并处理片段间通信。

创建一个名为 ActivityComs 的新类，并按照接下来所示的代码将其编码为我们的接口：

```
public interface ActivityComs {
    void onTitlesListItemSelected(int pos);

    void onTagsListItemSelected(String tag);
}
```

现在我们有两个方法可用于在 Fragment 类与 MainActivity 之间进行通信。其中一个（onTitlesListItemSelected）将传入一个 int，以表示数据库中所需 Photo 对象行的 _id。另一个（onTagsListItemSelected）将传入一个字符串，即要搜索并与 wis_table_photos 表中行匹配的标签。

我们将分三个阶段处理此问题：

- 1. 首先，我们将所需代码添加到 TitlesFragment。
- 2. 接下来，我们将代码添加到 TagsFragment。
- 3. 最后，我们将整个东西整合在一起，并在 MainActivity 中实现该接口。

由于我们将在现有方法中来回切换，同时也会编写新方法，如果您在任何地方感到困惑，只需打开 Chapter 24/Where Its Snap 3 文件夹中的相关文件，即可看到本章中我们添加的所有代码，并完整地放在相应的位置。

### 编写 TitlesFragment

我们离在照片应用中实际显示一张照片已经非常近了。我们需要从数据库加载照片标题列表到由 ListFragment 提供的 ListView 中，并处理用户单击列表项时发生的情况。

其中一些代码在完成 MainActivity 的编码之前可能无法完全理解，而 MainActivity 自然会处理 TitlesFragment 与 ViewFragment 之间的通信，以及 TagsFragment 与 TitlesFragment 之间的通信。

让我们向 TitlesFragment 添加两个新成员：一个用于加载数据的 Cursor 成员，以及我们新接口的一个实例。

在所示位置添加两个高亮的成员变量：

```
public class TitlesFragment extends ListFragment {
    private Cursor mCursor;
    private ActivityComs mActivityComs;

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
    }
}
```

接下来，在 onCreate 方法中，我们执行以下操作：

- 使用 getArguments().getString 从 Bundle 中获取表示标签的字符串，以便在数据库中搜索。
- 获取 DataManager 的实例。
- 如果 TitlesFragment 接收到要搜索的标签，它只会显示带有该匹配标签的照片标题，但如果未接收到标签，它将显示所有标题。考虑到这一点，if 块将使用 getTitles 或 getTitlesWithTag 将 Cursor 对象加载数据。
- 接下来，我们创建一个 SimpleCursorAdapter 实例。Cursor 适配器就像数组适配器，只是它使用 Cursor 对象。SimpleCursorAdapter 非常适合从 Cursor 在 ListView 中显示简单数据。其构造函数接受的参数列表看起来有些令人生畏，但并不像看起来那么糟糕。重要的参数是 ListView 中每个列表项的布局。list_item_1 默认提供，我们不需要创建它。接着是包含数据的 Cursor，然后是用于在 Cursor 中识别数据的方式，我们传递表中的列名。
- 然后，我们调用 setListAdapter 将新的 SimpleCursorAdapter 设置为 ListView 的适配器。

### 提示

要了解有关 SimpleCursorAdapter 的更多信息，请查看 Android 开发者网站：http://developer.android.com/reference/android/widget/SimpleCursorAdapter.html

将我们刚才讨论的以下高亮代码添加到 onCreate 中：

```
@Override
public void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);

    // Get the tag to search for
    String tag = getArguments().getString("Tag");

    // Get an instance of DataManager
    DataManager d = new DataManager(getActivity().getApplicationContext());

    if(tag == "NOTAG"){
    // Get all the titles from the database
    mCursor = d.getTitles();
    }else{
    // Get all the titles with a specific related tag
    mCursor = d.getTitlesWithTag(tag);
    }

    // Create a new adapter
    SimpleCursorAdapter cursorAdapter = new SimpleCursorAdapter(getActivity(),
    android.R.layout.simple_list_item_1, mCursor,
    new String[] { DataManager.TABLE_ROW_TITLE },
    new int[] { android.R.id.text1 }, 0 );

    // Attach the adapter to the ListView
    setListAdapter(cursorAdapter);
}
```

现在，我们可以处理用户在 ListView 中点击标题时发生的情况。我们实现 onListItemClick 方法，只需该方法的 position 参数即可完成我们的工作。

我们使用 Cursor 的 moveToPosition 方法并传入 position，将光标设置到下一行代码所需的正确位置。

然后我们声明并初始化一个名为 dBID 的 int 变量，其值来自数据库中的 _id 列，代码如下：

```
int dBID = mCursor.getInt(
mCursor.getColumnIndex(
DataManager.TABLE_ROW_ID));
```

最后，我们可以调用我们接口的 onTitlesListItemSelected 方法，将适当的 _id 值传递给 MainActivity。

将我们刚才讨论的 onListItemClick 方法添加到 TitlesFragment：

```
public void onListItemClick(ListView l, View v, int position, long id) {
    // Move the cursor to the clicked item in the list
    mCursor.moveToPosition(position);

    // What is the database _id of this item?
    int dBID = mCursor.getInt(
    mCursor.getColumnIndex(
    DataManager.TABLE_ROW_ID));

    // Use the interface to send the clicked _id
    mActivityComs.onTitlesListItemSelected(dBID);
}
```

当然，我们尚未初始化 mActivityComs 成员，我们会以第 19 章《使用多个片段》中的相同方式进行初始化，并以同样的方式将其设置为 null。

在适当的时间添加最后两个方法以实现此目的：

```
@Override
public void onAttach(Activity activity) {
    super.onAttach(activity);
    mActivityComs = (ActivityComs)activity;
}
```

```
@Override
public void onDetach() {
    super.onDetach();
    mActivityComs = null;
}
```

现在我们可以为 TagsFragment 编写代码，让它通过 MainActivity 与 TitlesFragment 通信。

### 编写 TagsFragment

TagsFragment 类需要做的就是从相应表中显示标签列表，当用户点击其中一个时，通过 ActivityComs 接口让 MainActivity 知道。

将 ActivityComs 接口作为新成员变量添加，如以下代码中高亮所示：

```
public class TagsFragment extends ListFragment {

    private ActivityComs mActivityComs;

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
    }
}
```

就像刚才编写 TitlesFragment 时所见，接下来的代码允许我们：

- 获取对数据库的访问权限
- 从 wis_table_tags 获取标签列表
- 设置 SimpleCursorAdapter 并将其绑定到 ListView

将我们刚才讨论的高亮代码添加到 onCreate 方法中：

```
@Override
public void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);

    DataManager d = new DataManager(getActivity().getApplicationContext());
    Cursor c = d.getTags();

    // Create a new adapter
    SimpleCursorAdapter cursorAdapter = new SimpleCursorAdapter(getActivity(),
    android.R.layout.simple_list_item_1, c,
    new String[] { DataManager.TABLE_ROW_TAG },
    new int[] { android.R.id.text1 }, 0);

    // Attach the Cursor to the adapter
    setListAdapter(cursorAdapter);
}
```

现在，处理触摸操作。这段代码执行以下操作：

- 通过链式调用 getAdapter 和 getCursor 从 SimpleCursorAdapter 获取 Cursor 对象，并将结果强制转换为 SimpleCursorAdapter
- 根据刚刚点击的标签创建一个字符串
- 使用接口的 onTagsListItemSelected 方法将字符串传递给 MainActivity

添加我们讨论过的 onListItemClick 方法：

```
public void onListItemClick(ListView l, View v,
int position, long id) {
// What tag has just been clicked?
Cursor c = ((SimpleCursorAdapter)l.getAdapter()).getCursor();
c.moveToPosition(position);

String clickedTag = c.getString(1);
// 1 is the position of the string
Log.e("clickedTag = ", "" + clickedTag);

mActivityComs.onTagsListItemSelected(clickedTag);
}
```

现在，我们需要使用通常的两种方法来处理 ActivityComs 实例的创建和销毁，如下所示：

```
@Override
public void onAttach(Activity activity) {
super.onAttach(activity);
mActivityComs = (ActivityComs)activity;
}
```

```
@Override
public void onDetach(
