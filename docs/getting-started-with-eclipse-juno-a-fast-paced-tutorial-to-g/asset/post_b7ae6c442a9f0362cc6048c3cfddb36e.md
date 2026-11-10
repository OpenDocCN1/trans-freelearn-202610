
We have created a constructor that receives a Contact object and also implemented a getter for it. We have also implemented the getToolTipText and getName methods to return the contact name. The equals method must also be implemented in order to make sure that only one editor is open for one contact. We have used the auto-generated method stub for the other methods. These methods were omitted in the code.

Now let's work on the contact editor. The following code is the implementation of the ContactEditor class:

```
public class ContactEditor extends EditorPart {

    private boolean dirty = false;
    private Text addressText;
    private Text nameText;
    private Text phoneText;

    @Override
    public void init(IEditorSite site, IEditorInput input)
        throws PartInitException {
        setSite(site);
        setInput(input);
    }

    @Override
    public void createPartControl(Composite parent) {
        parent.setLayout(new GridLayout(2, false));
        Label nameLabel = new Label(parent, SWT.NONE);
        nameLabel.setText("Name");
        nameLabel.setLayoutData(new GridData());
        nameText = new Text(parent, SWT.BORDER | SWT.FILL);
        nameText.setLayoutData(new GridData());

        Label addressLabel = new Label(parent, SWT.NONE);
        addressLabel.setText("Address");
        addressLabel.setLayoutData(new GridData());
        addressText = new Text(parent, SWT.BORDER);
        addressText.setLayoutData(new GridData());

        Label phoneLabel = new Label(parent, SWT.NONE);
        phoneLabel.setText("Phone number");
        phoneLabel.setLayoutData(new GridData());
        phoneText = new Text(parent, SWT.BORDER );
        phoneText.setLayoutData(new GridData());

        ContactEditorInput editorInput = (ContactEditorInput)
        getEditorInput();
        if(editorInput != null){
            Contact contact = editorInput.getContact();
            nameText.setText(contact.getName());
            this.setPartName(contact.getName());
            addressText.setText(contact.getAddress());
            phoneText.setText(contact.getPhoneNumber());
        }

        nameText.addModifyListener(new ContactEditorModifyListener());
        addressText.addModifyListener(new ContactEditorModifyListener());
        phoneText.addModifyListener(new ContactEditorModifyListener());
    }

    @Override
    public void doSave(IProgressMonitor monitor) {
        dirty = false;
        firePropertyChange(IWorkbenchPartConstants.PROP_DIRTY);

        ContactEditorInput editorInput = (ContactEditorInput)
        getEditorInput();
        if(editorInput != null){
            Contact contact = editorInput.getContact();
            contact.setName(nameText.getText());
            contact.setAddress(addressText.getText());
            contact.setPhoneNumber(phoneText.getText());

            this.setPartName(contact.getName());
        }
    }

    @Override
    public boolean isDirty() {
        return dirty;
    }

    @Override
    public void doSaveAs() {
    }

    @Override
    public boolean isSaveAsAllowed() {
        return false;
    }

    @Override
    public void setFocus() {
    }

    private class ContactEditorModifyListener implements ModifyListener
    {
        @Override
        public void modifyText(ModifyEvent e) {
            dirty = true;
            firePropertyChange(IWorkbenchPartConstants.PROP_DIRTY);
        }
    }
}
```

Notice that ContactEditor extends EditorPart instead of ViewPart, just like ContactListView did. It also contains a createPartControl method that must be implemented. Our implementation creates SWT elements that compose a basic form for the contact's fields. It also fills these fields if the current editor input is not null, which will happen when we edit a contact instead of creating a new one. There's also an inline class named ContactEditorModifyListener, which, as the name implies, gets notified whenever a text element is modified. We use this class to change the dirty status to true. The dirty status means that the editor's input contains unsaved changes. This state is represented by an asterisk character (*) in front of the editor's name. This state will also allow us to enable or disable the save button accordingly. Notice how we change the dirty status in the doSave method.

Now, let's register the ContactEditor class in the plugin.xml file. Add a new extension for the org.eclipse.ui.editors extension point and add an editor element with the properties shown in the following screenshot:

![](img/d7c3f2018603810fae83e1a6fd98749e_231_0.png)

Editors are not added to perspectives as regular views are. Editors are opened programmatically by calling the openEditor method of the currently active WorkbenchPage, as we will see later in this chapter. They can be opened in any perspective, and they are placed in the editor space, which lies in the middle of the window.

## The Save and New commands and menu entries

The next step is to implement commands, handlers, and menu entries that enable users to create new contacts and to save their work. Let's start with the handler classes. This is the code for the save command handler:

```
public class SaveContactHandler extends AbstractHandler {
    @Override
    public Object execute(ExecutionEvent event) throws ExecutionException {
        PlatformUI.getWorkbench().getActiveWorkbenchWindow().
        getActivePage().getActiveEditor().doSave(new NullProgressMonitor());
        return null;
    }
}
```

Note that there are two AbstractHandler classes, one in the org.eclipse.core.commands package and the other in org.eclipse.ui.commands. We'll be using the former because the latter is deprecated.

This is the new contact handler:

```
public class NewContactHandler extends AbstractHandler {

    @Override
    public Object execute(ExecutionEvent event) throws
ExecutionException {
        Contact contact = new Contact("New Contact", "", "");
        IViewPart view = PlatformUI.getWorkbench().
getActiveWorkbenchWindow().getActivePage().findView("ContactsList.ContactsListView");
        ((ContactListView) view).addEntry(contact);
        return null;
    }
}
```

The new contact handler simply creates a new Contact object and calls the addEntry method in the ContactsListView view.

Let's proceed with the necessary modifications in the plugin.xml file, which is similar to what was demonstrated in the previous chapter. Add an extension to the org.eclipse.ui.commands extension point and add command elements for the Save and New commands. In the example, we use contactsList.save and contactsList.new as IDs and Save and New as names. Now add the org.eclipse.ui.handlers extension, and create two handler elements for the commands, linking them to the handler classes we just created.

The next step is to add the menu entries for these commands. Let's add a File entry in the main menu with the New and Save entries and a Save button in the toolbar. Add the elements shown in the following screenshot to create them:

![](img/d7c3f2018603810fae83e1a6fd98749e_232_0.png)

The New and Save commands point to the contactsList.new and contactsList.save command IDs respectively.

If you run the application at this point, you will realize that the Save button in the toolbar is not there. In fact, there's no toolbar in our application! That's because the implementation of the ApplicationWorkbenchWindowAdvisor extension provided by the template disables the toolbar. Open this class, and either comment out the configurer.setShowCoolBar(false) call inside the preWindowOpen method or change the parameter to true to display the toolbar.

## Tying up the two views

Now that we have the two views and the menu entries, the next step is to connect the two views by opening an editor when the user double-clicks on an entry in the list. We will do that by adding a double-click listener to the ContactList view. Let's start by coding the listener. Create a class named ContactListDoubleClickListener with the following code:

```
public class ContactListDoubleClickListener implements
    IDoubleClickListener {

    @Override
    public void doubleClick(DoubleClickEvent event) {
        TableViewer viewer = (TableViewer) event.getSource();
        StructuredSelection selection = (StructuredSelection) viewer.getSelection();
        Contact contact = (Contact) selection.getFirstElement();
        try {
            PlatformUI.getWorkbench().getActiveWorkbenchWindow().getActivePage().openEditor(new ContactEditorInput(contact), "ContactsList.ContactEditor");
        } catch (PartInitException e) {
            e.printStackTrace();
        }
    }
}
```

When a IDoubleClickListener implementation is registered to a SWT widget, its doubleClick method is executed when the widget is double-clicked. Our doubleClick implementation gets the TableViewer widget and discovers which element was selected. This is another advantage of using content providers; they allow you to access the object to which the TableViewer entry refers easily.
