object and then we initialize our LayoutInflater member. Next, we use LayoutInflater to declare and initialize a new View from our pager_item.xml layout file.

After this, we get a reference to ImageView inside the pager_item.xml layout. We can now add the appropriate image as the content of ImageView is based on the position parameter of the instantiateItem method and the appropriate ID from the images array.

Finally, we add the layout to PagerAdapter with addView and return from the method.

Now, we will add the method we just discussed here:

```
```java
@Override
public Object instantiateItem(ViewGroup container, int position) {

    ImageView image;

    inflater = (LayoutInflater)
    context.getSystemService(Context.LAYOUT_INFLATER_SERVICE);
    View itemView = inflater.inflate(R.layout.pager_item,
    container,false);

    // get reference to imageView in pager_item layout
    image = (ImageView) itemView.findViewById(R.id.imageView);

    // Set an image to the ImageView
    image.setImageResource(images[position]);

    // Add pager_item layout as the current page to the ViewPager
    ((ViewPager) container).addView(itemView);

    return itemView;
}
```
```

The last method we must override is destroyItem, which the class can call when it needs to remove an appropriate item based on the value of the position parameter.

Add the destroyItem method after the previous code and before the closing curly brace of the ImagePagerAdapter class: