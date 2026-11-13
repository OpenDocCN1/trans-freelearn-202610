# # Coding the MainActivity class

Finally, we can code our MainActivity class. As with the ImagePagerAdapter class, for clarity, add the following import statements manually to the MainActivity.java class before the class declaration, as shown in the next code snippet:

```
```java
import android.support.v4.view.PagerAdapter;
import android.support.v4.view.ViewPager;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.Menu;
import android.view.MenuItem;
```
```

Now, we need a few member variables. Unsurprisingly, we need ViewPager, which will be used to get a reference to ViewPager in our layout, as well as a ImagePagerAdapter reference for the class that we have just coded. We also need an int array to hold an array of image IDs:

```
```java
public class MainActivity extends AppCompatActivity {

    ViewPager viewPager;
    PagerAdapter adapter;

    int[] images;

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

    ...
```
```

All the rest of the code goes in onCreate. We initialize our int array with each of the images that we added to the drawable-xhdpi folder.

We initialize ViewPager in the usual way with the findViewByID method. We also initialize ImagePagerAdapter by passing in a reference to MainActivity and the images array, as required by the constructor that we coded previously. Finally, we bind the adapter to the pager with setAdapter.

Add the highlighted code in the onCreate method:

```
```java
@Override
```
```