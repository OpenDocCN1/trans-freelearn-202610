ContentPage nor the Children property of Layout<T> (from which StackLayout derives) is a bindable property. Sometimes changes to nonbindable properties result in the PropertyChanged event being fired, but that’s not guaranteed. The PropertyChanged event is only guaranteed for bindable properties.

Later in this book you’ll see how to define your own bindable properties.

# Automated data bindings

As you’ve seen, it’s possible to install event handlers on the Entry and Editor to determine when the Text property changes, and to use that occasion to set the Title and Text properties of the Note class. Similarly, you can use the PropertyChanged event of the Note class to keep the Entry and Editor updated.

In other words, the NoteTaker4 program has paired up objects and properties so that they track each other’s values. As one property changes, the other is updated.

It turns out that tasks like this are very common, and this is why Xamarin.Forms allows such tasks to be automated with a technique called data binding.

Data bindings involve a source and a target. The source is the object and property that changes, and the target is the object and property that is changed as a result. But that’s a simplification. Although the distinction between target and source is clearly defined in any particular data binding, sometimes the properties affect each other in different ways: Sometimes the target causes the source to be updated, and sometimes the source and target update each other.

The data binding mechanism in Xamarin.Forms uses the PropertyChanged event to determine when a source property has changed. Therefore, a source property used in a data binding must be part of a class that implements INotifyPropertyChanged (either directly or through inheritance), and the class must fire a PropertyChanged event when that property changes. (Actually this is not entirely true: If the source property never changes, a PropertyChanged event is not required for the data binding to work, but it’s only a “one time” data binding and not very interesting.)

The target property of a data binding must be backed by a BindableProperty. As you’ll see, this requirement is imposed by the programming interface for data bindings. You cannot set a data binding without referencing a BindableProperty object!

In the NoteTaker5 program, the Text properties of Entry and Editor are the binding targets because they are backed by bindable properties named TextProperty. The Title and Text properties of the Note class are the binding sources. The Note class implements INotifyPropertyChanged so PropertyChanged events are fired when these source properties changes.

You can define a data binding to keep a target property updated with the value of a source property with two statements: The first statement associates the two objects by setting the BindingContext property of the target object to the source object. In this case that’s the instance of the Note class: