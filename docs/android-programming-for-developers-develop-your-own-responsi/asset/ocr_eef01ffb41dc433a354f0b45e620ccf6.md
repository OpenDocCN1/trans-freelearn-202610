# So, how does this heap thing help me?

Any UI element that has its id property set can have its reference retrieved from the heap using the findViewById method, which is part of the Activity, the AppCompatActivity class. As it is part of the class that we extend, we have access to it:

```
myButton = (Button) findViewById(R.id.myButton);
```

The preceding code assumes that myButton has been declared previously to an appropriate type; in this case, Button. Also, the preceding code assumes that within the XML layout, there is a button with an ID property set to myButton.

Note that findViewById is also polymorphic. We know this because we use a cast, (Button), to be explicit about making the returned object Button from its View parent type, just like we did with our object of the type Elephant with the feed method in the last chapter.

This is quite exciting because it implies that we can grab a reference to a whole bunch of stuff from our layout. We can then start using all the methods that these objects have. Here are some examples of the methods we can use for Button objects:

```
myButton.setText
myButton.setHeight
myButton.setOnClickListener
myButton.setVisibility
```

Remember that Button alone has around 50 methods! If you think that after nearly 10 chapters we are finally going to start doing some really neat stuff with Android, you would be right!