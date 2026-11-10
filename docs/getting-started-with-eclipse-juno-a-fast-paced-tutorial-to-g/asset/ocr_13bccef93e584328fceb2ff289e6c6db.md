# Saving the view's current state

It's important for most of the views to be able to save its state so that it can be retrieved when Eclipse is restarted. There is a saveState(IMemento memento) method in IViewPart that must be implemented in order to achieve that. If you take a look at ViewPart's implementation, though, you will be disappointed:

```
public void saveState(IMemento memento) {
    // do nothing
}
```

If you want your view to be capable of saving its own state, you have to override this method. Let's see how to do this.

We can see that the method receives a IMemento object as a parameter. This is the object we have to update in order to save the view's state. From the IMemento's JavaDoc, mementos are objects to which you can assign a "mapping of arbitrary string keys to primitive values, and by allowing mementos to have other mementos as children (arranged into a tree)". This mapping is created by calling the put<type>(String key, <type> value) methods, such as putFloat, putInteger, putString, and so on. Child mementos are created with the createChild method.

Once you have accordingly implemented the saveState method, Eclipse guarantees that the information stored in the memento object will be available across different Eclipse sessions, using an xml-based storage format under the hood. However, we still have to tell Eclipse how to use this information to recompose the view's state. This is done by overriding the init(IViewSite site, IMemento memento) method, since the ViewPart implementation simply ignores the memento object and delegates to init(IViewSite site). As a general rule, information from the memento is only saved in the current object, leaving the task of assigning the values back to the SWT elements to the createPartControl method. This happens because the platform calls init before createPartConrol.

# Adding context help to your view

The easiest way of getting help in Eclipse is through the Dynamic Help, which can be activated in Help | Dynamic Help. This opens a Help view which contains basic information and usage guidance for the currently focused view. Each view provides the help content, and the Eclipse platform takes care of assembling the help view.