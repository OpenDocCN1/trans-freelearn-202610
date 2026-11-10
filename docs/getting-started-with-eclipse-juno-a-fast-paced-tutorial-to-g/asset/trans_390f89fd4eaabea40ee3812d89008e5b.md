
我们创建了一个接收 Contact 对象的构造函数，并为其实现了 getter。我们还实现了 getToolTipText 和 getName 方法以返回联系人姓名。为了确保对同一个联系人只打开一个编辑器，还必须实现 equals 方法。我们已为其他方法使用了自动生成的代码存根。这些方法在代码中被省略了。

现在让我们来处理联系人编辑器。以下代码是 ContactEditor 类的实现：

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

请注意 ContactEditor 扩展的是 EditorPart 而不是 ViewPart，就像 ContactListView 一样。它还包含必须实现的 createPartControl 方法。我们的实现创建了 SWT 元素，组成了联系人字段的基础表单。如果当前编辑器输入不为 null，也会填充这些字段，这将在编辑联系人而不是创建新联系人时发生。还有一个名为 ContactEditorModifyListener 的内部类，其名称暗示它会在文本元素被修改时收到通知。我们使用此类将 dirty 状态更改为 true。dirty 状态表示编辑器的输入包含未保存的更改。此状态在编辑器名称前用星号字符 (*) 表示。此状态还允许我们相应地启用或禁用保存按钮。请注意我们在 doSave 方法中如何更改 dirty 状态。

现在，让我们在 plugin.xml 文件中注册 ContactEditor 类。为 org.eclipse.ui.editors 扩展点添加一个新的扩展，并添加一个编辑器元素，其属性如以下屏幕截图所示：

![](img/d7c3f2018603810fae83e1a6fd98749e_231_0.png)

编辑器不会像普通视图那样添加到透视图中。编辑器是通过调用当前活动 WorkbenchPage 的 openEditor 方法以编程方式打开的，如本章后面将看到的那样。它们可以在任何透视图中打开，并放置在编辑器区域中，该区域位于窗口中间。

## 保存和新建命令及菜单项

下一步是实现命令、处理器和菜单项，以使用户能够创建新联系人并保存工作。让我们从处理器类开始。这是保存命令处理器的代码：

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

请注意有两个 AbstractHandler 类，一个在 org.eclipse.core.commands 包中，另一个在 org.eclipse.ui.commands 中。我们将使用前者，因为后者已弃用。

这是新建联系人处理器：

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

新建联系人处理器 simply 创建一个新的 Contact 对象并在 ContactsListView 视图中调用 addEntry 方法。

让我们继续在 plugin.xml 文件中进行必要的修改，这与上一章演示的类似。为 org.eclipse.ui.commands 扩展点添加一个扩展，并为保存和新建命令添加命令元素。在示例中，我们使用 contactsList.save 和 contactsList.new 作为 ID，使用 Save 和 New 作为名称。现在添加 org.eclipse.ui.handlers 扩展，并为命令创建两个处理器元素，将它们链接到我们刚刚创建的处理器类。

下一步是为这些命令添加菜单项。让我们在主菜单中添加一个 File 条目，其中包含新建和保存条目，并在工具栏中添加一个保存按钮。添加以下屏幕截图中显示的元素以创建它们：

![](img/d7c3f2018603810fae83e1a6fd98749e_232_0.png)

新建和保存命令分别指向 contactsList.new 和 contactsList.save 命令 ID。

如果此时运行应用程序，你会发现工具栏中的保存按钮不存在。事实上，我们的应用程序中根本没有工具栏！这是因为模板提供的 ApplicationWorkbenchWindowAdvisor 扩展的实现禁用了工具栏。打开此类，并在 preWindowOpen 方法内部注释掉 configurer.setShowCoolBar(false) 调用，或将参数更改为 true 以显示工具栏。

## 连接两个视图

现在我们有了两个视图和菜单项，下一步是通过在用户在列表中双击条目时打开编辑器来连接这两个视图。我们将通过向 ContactList 视图添加双击监听器来实现这一点。让我们开始编写监听器。创建一个名为 ContactListDoubleClickListener 的类，代码如下：

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

当 IDoubleClickListener 实现注册到 SWT 小部件时，其 doubleClick 方法会在小部件被双击时执行。我们的 doubleClick 实现获取 TableViewer 小部件并发现选择了哪个元素。这也是使用内容提供程序的另一个优点；它们允许你轻松访问 TableViewer 条目所引用的对象。
