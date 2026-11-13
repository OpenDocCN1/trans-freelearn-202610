
请注意，rawQuery 方法返回一个游标类型的对象。

## 提示

我们与 SQLite 交互有多种不同的方式，各有优缺点。这里，我们选择使用原始 SQL 语句，因为这样做完全透明，我们知道自己在做什么，同时还能加强对 SQL 语言的了解。如果想了解更多，请参阅下一个提示。

## 数据库游标

除了让我们访问数据库的类和允许我们执行查询的方法之外，还有一个问题，即我们从查询中获取的结果究竟以何种格式返回。

幸运的是，有 cursor 类。我们的所有数据库查询都将返回 Cursor 类型的对象。我们可以使用 Cursor 类的方法，像在这段代码中使用的那样，有选择地访问查询返回的数据：

```
Log.i(c.getString(1), c.getString(2));
```

之前的代码会将查询返回结果中前两列存储的两个值输出到 logcat。正是 Cursor 对象本身决定了我们当前将读取返回数据中的哪一行。

我们可以访问 Cursor 对象的许多方法，包括 moveToNext 方法，它不出所料地会将 Cursor 移动到下一可读行：

```
c.moveToNext();
```

```
/*
 * This same code now outputs the data in the
 * first and second column of the returned
 * data but from the SECOND row.
 */
```

```
Log.i(c.getString(1), c.getString(2));
```

在某些情况下，我们将能够将 Cursor 绑定到我们 UI 的一部分（例如 ListView），并将一切交给 Android API。当我们在下一章为 Where it's snap 应用添加数据库时，将看到此过程。

Cursor 类还有更多有用的方法，我们很快就会看到其中一些。

### 提示

对 Android SQLite API 的这篇介绍其实只是触及了其功能的表面。随着我们深入学习，还会遇到更多方法和类。然而，如果你的应用构想需要复杂的数据管理，深入学习是值得的。如果你想了解更多，我推荐你阅读 Jason Wei 所著的《Android 数据库编程》（Packt Publishing 出版），你可以在 https://www.packtpub.com/application-development/android-database-programming 找到它。

现在，我们可以看到所有这些理论是如何结合在一起的，以及我们将如何在实际可用的小型应用中构建数据库代码。

## 数据库小型应用

在本节中，我们将付诸实践迄今所学的一切，并创建一个小型数据库，使我们能够添加、删除、搜索和选择数据。

首先，让我们创建一个新项目，所有设置均使用默认值，并将其命名为 Database。此小型应用的代码位于 Chapter 23/Database 文件夹的下载包中。

## 实现布局

通过复制并粘贴下一块 XML 代码来创建布局。代码中没有什么新的或复杂的内容，但请务必审查它，记住 EditText 和 Button 小部件上的各种 id 属性，因为这将使理解 Java 代码更加简单。

将此 XML 添加到 layout_main.xml 文件中：

```
<RelativeLayout
xmlns:android="http://schemas.android.com/apk/res/android"
xmlns:tools="http://schemas.android.com/tools"
android:layout_width="match_parent"
android:layout_height="match_parent"
android:paddingLeft="@dimen/activity_horizontal_margin"
android:paddingRight="@dimen/activity_horizontal_margin"
android:paddingTop="@dimen/activity_vertical_margin"
android:paddingBottom="@dimen/activity_vertical_margin"
tools:context=".MainActivity">

<EditText
android:layout_width="wrap_content"
android:layout_height="wrap_content"
android:id="@+id/editName"
android:hint="Type a name"
android:layout_alignParentTop="true"
android:layout_centerHorizontal="true" >

<EditText
android:layout_width="wrap_content"
android:layout_height="wrap_content"
android:id="@+id/editAge"
android:hint="Type their age"
android:layout_below="@+id/editName"
android:layout_alignLeft="@+id/editName"
android:layout_alignStart="@+id/editName" >

<Button
android:layout_width="wrap_content"
android:layout_height="wrap_content"
android:text="INSERT"
android:id="@+id/btnInsert"
android:layout_below="@+id/editAge"
android:layout_toRightOf="@+id/editSearch"
android:layout_toEndOf="@+id/editSearch"
android:layout_marginTop="24dp" >

<Button
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="DELETE"
    android:id="@+id/btnDelete"
    android:layout_above="@+id/btnSearch"
    android:layout_alignParentRight="true"
    android:layout_alignParentEnd="true"
    android:layout_marginBottom="40dp" />

<Button
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="SELECT *"
    android:id="@+id/btnSelect"
    android:layout_alignParentBottom="true"
    android:layout_alignRight="@+id/editName"
    android:layout_alignEnd="@+id/editName" >

<Button
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Search"
    android:id="@+id/btnSearch"
    android:layout_above="@+id/btnSelect"
    android:layout_alignLeft="@+id/btnDelete"
    android:layout_alignStart="@+id/btnDelete"
    android:layout_marginBottom="32dp" >

<EditText
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:id="@+id/editSearch"
    android:hint="name to search"
    android:layout_alignBottom="@+id/btnSearch"
    android:layout_alignParentLeft="true"
    android:layout_alignParentStart="true" >

<EditText
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:id="@+id/editDelete"
    android:hint="name to delete"
    android:layout_alignBottom="@+id/btnDelete"
    android:layout_alignParentLeft="true"
    android:layout_alignParentStart="true"
    android:focusableInTouchMode="true" >
</RelativeLayout>
```

以下是 Android Studio 设计器中完成的布局：

![](img/35d01831c19688a44e50bce7c489007a_758_0.png)

## 编写数据库类

我们将通过实现 SQLiteOpenHelper 来创建一个管理数据库的类。它还将定义一些 final 字符串，用于表示表及其列的名称。此外，它将提供一系列帮助方法，我们可以调用这些方法来执行所有必要的查询。在必要时，这些帮助方法将返回一个 Cursor 对象，我们可以在 MainActivity 中使用它来显示检索到的数据。然后，如果我们的应用需要发展，添加新的帮助方法也将非常容易。

创建一个名为 DataManager 的新类，并添加以下成员变量：

```
public class DataManager {
    // This is the actual database
    private SQLiteDatabase db;

    /*
     Next we have a public static final string for
     each row/table that we need to refer to both
     inside and outside this class
     */

    public static final String TABLE_ROW_ID = "id";
    public static final String TABLE_ROW_NAME = "name";
    public static final String TABLE_ROW_AGE = "age";

    /*
    Next we have a private static final strings for
    each row/table that we need to refer to just
    inside this class
    */

    private static final String DB_NAME = "address_book_db";
    private static final int DB_VERSION = 1;
    private static final String TABLE_N_AND_A =
     "names_and_addresses";
}
```

接下来，我们添加构造函数，它将创建我们自定义版本的 SQLiteOpenHelper 的实例。我们很快将把此类实现为内部类。构造函数还将初始化 db 成员，即我们的 SQLiteDatabase 引用。

将刚才讨论的以下构造函数添加到 DataManager 类中：

```
public DataManager(Context context) {
    // Create an instance of our internal //CustomSQLiteOpenHelper
    class
    CustomSQLiteOpenHelper helper = new
    CustomSQLiteOpenHelper(context);

    // Get a writable database
    db = helper.getWritableDatabase();
}
```

现在，我们可以添加将从 MainActivity 访问的帮助方法。首先是 insert 方法，它将基于传入方法的 name 和 age 参数执行 INSERT SQL 查询。

将 insert 方法添加到 DataManager 类中：

```
// Here are all our helper methods

// Insert a record
public void insert(String name, String age){
    // Add all the details to the table
    String query = "INSERT INTO " + TABLE_N_AND_A + " (" +
            TABLE_ROW_NAME + ", " +
            TABLE_ROW_AGE + ") " +
            "VALUES (" +
            "'" + name + "'" + ", " +
            "'" + age + "'" +
            ");";
    
    Log.i("insert() = ", query);
    
    db.execSQL(query);
}
```

这个名为 delete 的下一个方法将在数据库中删除一条记录，如果它在 name 列中具有与 name 参数中传递的值匹配的值。它使用 SQL DELETE 关键字来实现这一点。

将 delete 方法添加到 DataManager 类中：
