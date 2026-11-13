

@Override
public void destroyItem(ViewGroup container, int position, Object object) {
	// Remove pager_item layout from ViewPager
	((ViewPager) container).removeView((RelativeLayout) object);
}

正如我们在编写 ImagePagerAdapter 时所看到的，这非常简单。只需正确实现 instantiateItem 方法，并添加 PagerAdapter 类使用的简单重写方法，即可帮助幕后操作顺利运行。

## 编写 MainActivity 类

最后，我们可以编写 MainActivity 类。与 ImagePagerAdapter 类一样，为清晰起见，请在类声明之前手动将以下导入语句添加到 MainActivity.java 类中，如下一个代码片段所示：

```java
import android.support.v4.view.PagerAdapter;
import android.support.v4.view.ViewPager;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.Menu;
import android.view.MenuItem;
```

现在，我们需要一些成员变量。不出所料，我们需要 ViewPager，它将用于获取布局中 ViewPager 的引用，以及我们刚刚编写的类的 ImagePagerAdapter 引用。我们还需要一个 int 数组来保存图像 ID 数组：

```java
public class MainActivity extends AppCompatActivity {

    ViewPager viewPager;
    PagerAdapter adapter;

    int[] images;

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
```

其余所有代码都放在 onCreate 中。我们使用添加到 drawable-xhdpi 文件夹中的每张图像来初始化 int 数组。

我们以通常的方式使用 findViewById 方法初始化 ViewPager。我们还通过传入 MainActivity 的引用和 images 数组来初始化 ImagePagerAdapter，如我们之前编写的构造函数所要求的。最后，我们使用 setAdapter 将适配器绑定到视图页。

在 onCreate 方法中添加高亮代码：

```java
public void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);

    setContentView(R.layout.activity_main);

    // reference the images and put them in our array
    images = new int[] { R.drawable.image1,
        R.drawable.image2,
        R.drawable.image3,
        R.drawable.image4,
        R.drawable.image5,
        R.drawable.image6 };

    // get a reference to the ViewPager in the layout
    viewPager = (ViewPager) findViewById(R.id.pager);

    // Initialize our PagerAdapter
    adapter = new ImagePagerAdapter(MainActivity.this, images);

    // Bind the PagerAdapter to the ViewPager
    viewPager.setAdapter(adapter);
}
```

现在，我们可以运行应用了。

## 运行画廊应用

以通常的方式在模拟器或真实设备上运行应用。使用真实设备而不是模拟器会更令人满意，因为您可以用手指进行真正的滑动操作，而不是使用鼠标模拟滑动(拖动)。在此截图中，我们可以看到 int 数组中的第一张图像：

![](img/35d01831c19688a44e50bce7c489007a_678_0.png)

向右和向左轻轻滑动，以查看图像过渡的平滑悦目方式：

![](img/35d01831c19688a44e50bce7c489007a_679_0.png)

现在，我们将构建一个功能几乎相同的应用，只不过分页器中的每个页面将是一个 Fragment，它可以具有常规 Fragment 所能拥有的任何或全部功能，因为它们就是常规 Fragment。

## 构建 Fragment 分页器/滑块应用

我们可以将完整的 Fragment 放在 PagerAdapter 中作为页面。这非常强大，因为众所周知，一个 Fragment 可以拥有大量功能，甚至是功能齐全的 UI。

为保持代码简短直截了当，我们将在每个 Fragment 布局中添加一个 TextView，仅为了演示分页器正在工作。不过，当我们看到获取 TextView 引用是多么容易时，应该清楚如何轻松添加迄今所学的任何布局，然后让用户与之交互。

我们要做的第一件事是为滑块构建内容。当然，在这里，内容就是 Fragment。我们将构建一个名为 SimpleFragment 的简单类和一个名为 fragment_layout 的非常简单的布局。

这意味着每个幻灯片的外观将相同，但我们将使用实例化时 FragmentManager 传入的 Fragment ID 作为唯一 TextView 的文本。这样，当我们翻页/滑动浏览 Fragment 时，就会清楚每个 Fragment 都是一个新的独立实例。

当我们看到从列表加载 Fragment 的代码时，将非常简单地设计完全不同的 Fragment 类，就像我们之前做过的那样，并使用这些不同的类作为部分或全部幻灯片。当然，这些类中的每个类还可以使用不同的布局。

## 编写 SimpleFragment 类

与图片滑块应用一样，哪些类需要由 Android Studio 自动导入并不完全明确。我们使用这些类是因为它们彼此兼容，而且如果你让 Android Studio 建议导入哪些类，Android Studio 可能会“搞错”。项目文件位于 Chapter 20/Fragment Pager 文件夹中。

创建一个名为 Fragment Slider 的新项目，并将所有设置保留为默认值。

现在，创建一个名为 SimpleFragment 的新类，使其扩展 Fragment，并添加导入语句和一个成员变量，如以下代码片段所示：

```java
import android.os.Bundle;
import android.support.v4.app.Fragment;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.TextView;

public class SimpleFragment extends Fragment {
    // Holds the fragment id passed in when created
    public static final String MESSAGE = "";
}
```

我们需要添加两个方法。第一个是 newInstance，我们将从 MainActivity 调用它来设置并返回对 Fragment 的引用。接下来的代码做了我们之前见过的事情。它创建类的一个新实例，但也将一个 String 放入 Bundle 对象中，该对象最终将从 onCreate 方法读取。添加到 Bundle 的 String 是传递给此 newInstance 方法的唯一参数。

将 newInstance 方法添加到 SimpleFragment 类：

```java
// Our newInstance method which we call to make a new Fragment
public static SimpleFragment newInstance(String message)
{
    // Create the fragment
    SimpleFragment fragment = new SimpleFragment();
    // Create a bundle for our message/id
    Bundle bundle = new Bundle(1);
    // Load up the Bundle
    bundle.putString(MESSAGE, message);
    // Call setArguments ready for when onCreate is called
    fragment.setArguments(bundle);
    return fragment;
}
```

在 SimpleFragment 类的最后一个方法中，我们需要重写 onCreateView，在那里，像往常一样，我们将获取传入布局的引用，并将 fragment_layout 加载为布局。

然后，第一行代码使用 getArguments.getString 和键值对的 MESSAGE 标识符从 Bundle 中解包 String。

添加我们刚刚讨论的 onCreateView 方法：

```java
@Override
public View onCreateView(LayoutInflater inflater, ViewGroup
container, Bundle savedInstanceState) {
    // Get the id from the Bundle
    String message = getArguments().getString(MESSAGE);

    // Inflate the view as normal
    View view = inflater.inflate(R.layout.fragment_layout, container,
    false);

    // Get a reference to textView
    TextView messageTextView =
    (TextView)view.findViewById(R.id.textView);

    // Display the id in the TextView
    messageTextView.setText(message);

    // We could also handle any UI
    // of any complexity in the usual way
    // ..
    // ..

    return view;
}
```

