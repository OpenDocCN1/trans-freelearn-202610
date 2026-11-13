Next, we have the three arguments. Let's look at the first two now:

```
(LayoutInflater inflater, ViewGroup container, ...
```

We need LayoutInflater as we cannot call setContentView because Fragment provides no such method. In the body of onCreateView, we use the inflate method of inflater to inflate our layout contained in fragment_layout.xml and initialize view (an object of the type View) with the result.

We use container that was passed in to onCreateView as an argument in the inflate method. The container variable is a reference to the layout in activity_main.xml.

It might seem obvious that activity_main.xml is the containing layout, but as we will see later in the chapter, the ViewGroup container argument allows any Activity with any layout to be the container for our fragment. This is exceptionally flexible and makes our Fragment code reusable to a significant extent. The third argument that we pass into inflate is false, which means that we don't want our layout to be immediately added to the containing layout. We will do this soon from another part of the code.

The third argument of onCreateView is Bundle savedInstanceState, which is there to help us maintain the data that our fragments hold. We will see this in action in Chapter 22, Capturing Images.

Now that we have an inflated layout contained in view, we can use this to get a reference to Button like this:

```
myButton = (Button) view.findViewById(R.id.button);
```

And we can also use it as the return value to the calling code, as required:

```
return view;
```

Now, we can add an anonymous class to listen for clicks on our button in the usual manner. In the onClick method, we display a pop-up Toast message to demonstrate that everything is working as expected. Add this code just before the return statement in onCreateView as highlighted in this next code:

```
@Override
public View onCreateView(LayoutInflater inflater, ViewGroup
```