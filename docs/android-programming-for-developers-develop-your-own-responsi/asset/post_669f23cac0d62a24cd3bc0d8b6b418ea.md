
## All Android UI elements are classes too

When our app is run and the setContentView method is called from onCreate, the layout is inflated from XML UI classes and loaded into memory as usable objects. They are stored in a part of the DVM memory called the heap.

## Reintroducing references

However, where are all these UI objects/classes? We certainly can't see them in our code. And how on earth do we get our hands on them?

The DVM inside every Android device takes care of memory allocation to our apps. In addition, it stores different types of variables in different places.

Variables that we declare and initialize in methods are stored in the area of memory that is known as the stack. We can stick to our existing warehouse analogy when talking about the stack—almost. We already know how we can manipulate variables on the stack. So, let's talk about the heap and what is stored there.

### Note

Important fact: All objects of classes are reference type variables and are just references to the actual objects that are stored on the heap; they are not actual objects.

Think of the heap as a separate area of the same warehouse. The heap has lots of floor space for odd-shaped objects, racks for smaller objects, lots of long rows with small-sized cubby holes, and so on. This is where objects are stored. The problem is we have no direct access to the heap. Let's look at what exactly a reference variable is.

It is a variable that we refer to and use via a reference. A reference can be loosely, but usefully, defined as an address or a location. The reference (address or location) of the object is on the stack. So, when we use the dot operator, we are asking Dalvik to perform a task at a specific location, a location that is stored in the reference.

### Tip

Reference variables are just that—a reference. A way to access and manipulate the object (variables and methods), but they are not the actual variable itself.

Why, oh why would we ever want a system like this? Just give me my objects on the Stack already. Here is why.

## A quick break to throw out the trash

This is what the whole stack and heap thing does for us. As we know, the DVM keeps track of all our objects for us and stores them in a special area of our warehouse called the heap. Regularly, while our app is running, the DVM will scan the stack, the regular racks of our warehouse, and match up references to objects that are on the heap. And it destroys any objects that it finds without a matching reference. Or in Java terminology, it collects garbage.

Think of a very discriminating refuse vehicle driving through the middle of our heap, scanning objects to match up references (on the stack). No reference, you're garbage now. After all, if an object has no reference variable, we can't possibly do anything with it anyway because we have no way to access it. This system of garbage collection helps our apps run more efficiently by freeing up unused memory.

If this task was left to us, our apps would be much more complicated to code.

So, the variables declared in a method are local, on the stack, and only visible within the method they were declared in. A member variable (in an object) is on the heap and can be referenced from anywhere there is a reference to it and from anywhere that the access specification (encapsulation) allows.

## Stack and heap – a quick summary

Let's take a quick look at what you learned about a stack and heap:

- You don't delete objects, but the VM sends the garbage collector when it thinks it's appropriate. This is usually when there is no active reference to the object.
- Local variables and methods are on the stack, and the local variables are local to the specific method within which they were declared.
- Instance/class variables are on the heap (with their objects), but the reference to the object, the address, is a local variable on the stack.
- We control what goes onto the stack. We can use the objects on the heap, but only by referencing them.
- The heap is maintained by the garbage collector.
- An object is garbage collected when there is no longer a valid reference to it. So, when a reference variable, either local or instance, is removed from the stack, then its related object becomes eligible for garbage collection. And when the DVM decides the time is right (usually very promptly), it will free up the RAM memory to avoid running out of memory.
- If we try to reference an object that doesn't exist, we will get a NullPointerException error and the app will crash.

## So, how does this heap thing help me?

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

## Using Button and TextView widgets from our layout

To follow along with this mini project, create a new Android Studio project, call it Java Meet UI, and leave all the other options at their default values. As usual, you can find the Java code and the XML layout code in the Chapter 10/Java Meet UI folder.

First, let's build a simple UI:

- 1. Delete the autogenerated TextView widget, the one that reads "Hello world!".
- 2. Add a Large Text widget on the top and at the center of the layout.
- 3. Set its text property to 0 and its id property to txtValue. Pay careful attention to the case of the ID. It has an uppercase v.
- 4. Now, drag-and-drop six buttons on the layout so that it looks fairly similar to this next screenshot. The exact layout isn't important:

![](img/35d01831c19688a44e50bce7c489007a_346_0.png)

- 5. Double-click on each button in turn (left to right and then top to bottom) and set the text and id properties as shown in the next table. When you see the pop-up message as shown in the next figure, choose Yes each time:

![](img/35d01831c19688a44e50bce7c489007a_347_0.png)

| The text property | The id property |
|---|---|
| add | btnAdd |
| take | btnTake |
| grow | btnGrow |
| shrink | btnShrink |
| Hide | btnHide |
| Reset | btnReset |

When you're done, your layout should look as shown in the next screenshot:

![](img/35d01831c19688a44e50bce7c489007a_348_0.png)

The precise position and text on the buttons is not very important, but the values given to the id properties must be exactly the same. The reason for this is we will be using these IDs to get a reference to the buttons and TextView in this layout from our Java code.

Switch to the **MainActivity.java** tab in the editor and find the following code:

```
public class MainActivity extends AppCompatActivity{
```

Amend the preceding line to:

```
public class MainActivity extends AppCompatActivity implements
View.OnClickListener{
```

Note that the entire line we just amended is underlined in red, indicating an error. Now, because we have made MainActivity into OnClickListener by adding it as an interface, we must implement the abstract method of onClickListener. The method is called onClick. When we add the method, the error will be gone.

We can get Android Studio to add it for us by clicking anywhere on the line with an error and then using the keyboard combination Alt + Enter. After this, click on the **Implement methods** option, as shown in the next figure:

![](img/35d01831c19688a44e50bce7c489007a_349_0.png)

Now, click on **OK** to confirm that we want Android Studio to add the onClick method. The error goes away and we can carry on with adding code. We also have an onClick method, and we will soon see what we will do with that.

Now, we will declare an int variable called value and initialize it to 0. We will also declare six Button objects and a TextView object. We will give them the same names that we gave the id property values in our UI layout. This name association is not required, but it is useful to keep track of which Button in our Java code will be holding a reference to which Button from our UI.

Furthermore, we are declaring them all with the private access specification because we know that they will not be needed outside of this class.

Before you go ahead and type the code, note that all these variables are members of the MainActivity class. This means that we enter all the code that is shown next immediately after the class declaration that we amended in the previous step.
