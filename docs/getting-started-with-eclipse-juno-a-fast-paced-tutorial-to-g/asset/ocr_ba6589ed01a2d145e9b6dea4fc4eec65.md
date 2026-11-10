Standard constructors are used to create a widget. As seen in the code snippet of the *Setting up* section , they are explicitly disposed when there is no more need for them. In a different matter from traditional Java classes, you can note this behavior because widgets use operating systems resources that must be manually disposed in order to avoid resource leaks. The Java garbage collector does not dispose operating system resources.

In this context, when an SWT object is created, or when an SWT resource-based object is called, there is a need to manually dispose it. If there the resource has a parent, disposing it also causes the disposal of its children.

A group of widgets instances are used to build a user interface in the SWT. For that we have the control element, that is a user interface contained inside an application window, called a Shell. Several ordinary elements as Buttons, Texts, Labels and Tables subclass the abstract class Control.

To deal with actions, instead of sub-classing to implement code that reacts to modifications in a GUI, programmers are encouraged to use events and listeners that are added to widgets appropriately. Listeners are used to notify the program when an event has happened.

The last main feature that you must be aware of a widget, regards its application data. Due to the discouraged use of sub-classing (due to possible misbehavior regarding modifications in the superclass followed by the non-recompilation of the subclass), application data can be used to associate any object with a widget. The setData and getData methods can be used for that purpose, as we exemplify as follows:

```
widget.setData("Doctor", "Walter Bishop");
if ("Walter Bishop".equals(widget.getData("Doctor"))) {
    System.out.println("I'm perfectly sane!");
}
```

In this example, we store the data as a key/value pair in the widget, where the key is used to get/find the value object, that is, the Doctor key corresponds to the Walter Bishop object, and if the if statement returns true, the I'm perfectly sane message is printed.

Real-world situations may require factories to facilitate the storage of data, as it can be seen in the *Widgets* section.