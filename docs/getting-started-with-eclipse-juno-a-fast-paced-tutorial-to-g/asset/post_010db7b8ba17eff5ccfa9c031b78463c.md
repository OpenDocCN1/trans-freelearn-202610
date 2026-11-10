
We show an example of how to use a combo box, followed by the GUI output.

```
Display display = new Display ();
Shell shell = new Shell (display);
Combo combo = new Combo (shell, SWT.SIMPLE | SWT.READ_ONLY);
combo.setItems(new String [] {"Three", "Six", "Eight"});
combo.setSize(200, 50);
shell.pack ();
shell.open ();
while (!shell.isDisposed ()) {
    if (!display.readAndDispatch ()) display.sleep ();
}
display.dispose ();
```

In this example, a combo box is created using the simple style merged with the read-only one. Three items are added and no event is being handled.

![](img/d7c3f2018603810fae83e1a6fd98749e_114_0.png)

## Layouts

Layouts are classes to facilitate the positioning and sizing of controls inside a top-level window. They are platform independent, and do not share the same behavior as widgets.

The use of layouts is not mandatory when developing a SWT application; nonetheless, their use is highly encouraged. Layouts are a way to facilitate managing and positioning of real-world applications controls, because the use of setSize and setLocation methods in large programs can be a burden.

Layouts offer some benefits when compared to the positioning by hand. They often have the ability to compute the initial size of controls and to reposition them when a resizing operation occurs.

There are two types of the layout classes:
- A layout class that provides a specific API to its implementation
- A layout class that works with a specific layout data algorithm to be assigned for each child control of a composite

A layout data follows an algorithm that makes use of additional information on how the control should be positioned. When using this type of layout, we need to associate the composite with the layout class, and the composite children with the layout data.

Regardless, each layout presents a different underlying algorithm, and in the following sections we describe the available SWT layouts.

### FillLayout

The simplest layout class is FillLayout, which does not work along with a layout data. It positions the controls in a single row or column using the available space, and divides and assigns the same space for each one. If nothing is specified, it assumes that the controls will be as high as the highest one, and as wide as the widest. FillLayout is often used when the application has one child, as it will completely fill the shell.

FillLayout provides the following public fields:
- `type`: Layout orientation, that can be `SWT.HORIZONTAL` or `SWT.VERTICAL`
- `marginWidth`: Number of pixels of the margin to be placed along the left and right edges of the layout
- `marginHeight`: Number of pixels of the margin to be placed along the top and bottom edges of the layout
- `spacing`: Number of pixels responsible for the space among controls

We show an example of how to position 10 elements of a Text widget as follows. Note that when the application is resized, the controls are also resized by FillLayout.

```java
Display display = new Display();
Shell shell = new Shell(display);
shell.setLayout(new FillLayout());
for (int i=0; i<10; i++) {
    Text text = new Text(shell, SWT.BORDER);
    text.setText("Fill" + i);
}
shell.pack();
shell.open();
while (!shell.isDisposed()) {
    if (!display.readAndDispatch()) display.sleep();
}
display.dispose();
```

The output of this code snippet is shown as follows:

![](img/d7c3f2018603810fae83e1a6fd98749e_116_0.png)

The following image shows the resize behavior:

![](img/d7c3f2018603810fae83e1a6fd98749e_116_1.png)

### RowLayout

RowLayout positions the controls in a row (horizontally) or in a column (vertically), depending on how it was instantiated. It differs from FillLayout, as it does not fill out all the available space and it presents a layout data class called RowData to configure each of the controls individually. It is possible to specify their height and width when positioning them in the composite.

The following public fields are available:
- center: Specifies whether the control should be centered
- fill: Specifies whether all controls in a row should be of the same height for horizontal layouts or the same width for vertical layouts
- justify: Specifies if any remaining space should be allocated among the controls
- marginBottom: Specifies the number of pixels at the bottom edge of the layout
- marginHeight: Specifies the number of pixels at the top and bottom edges of the layout
- marginLeft: Specifies the number of pixels at the left edge of the layout
- marginRight: Specifies the number of pixels at the right edge of the layout
- marginTop: Specifies the number of pixels at the top edge of the layout
- marginWidth: Specifies the number of pixels at the left and right edges of the layout
- `pack`: Specifies whether all controls can take their preferred size
- `spacing`: Specifies the number of pixels among the edges of the cells
- `type`: Specifies if the layout is using rows or columns
- `wrap`: Specifies if a control should be wrapped to the next row if there is no space available

RowLayout can be used to position controls both in columns and in rows. Despite its name, if the programmer sets the field to SWT.VERTICAL, the layout orientation will follow this pattern.

In the following example, we show how to position a few buttons using RowLayout and RowData. In the following example, note that in the later two buttons, we use the RowData class to specify the size of the buttons by defining the height and the width:

```
Display display = new Display();
Shell shell = new Shell(display);
RowLayout layout = new RowLayout(SWT.HORIZONTAL);
layout.wrap = true;
layout.fill = false;
layout.justify = true;
shell.setLayout(layout);
Button b = new Button(shell, SWT.PUSH);
b.setText("Button 1");
b = new Button(shell, SWT.PUSH);
b.setText("Button 2");
RowData data = new RowData();
b.setLayoutData(data);
b = new Button(shell, SWT.PUSH);
b.setText("150 vertical");
data = new RowData();
data.height = 150;
b.setLayoutData(data);
b = new Button(shell, SWT.PUSH);
b.setText("150 horizontal");
data = new RowData();
data.width = 150;
b.setLayoutData(data);
shell.pack();
shell.open();
while (!shell.isDisposed()) {
    if (!display.readAndDispatch())
        display.sleep();
}
display.dispose();
```

The output of the sample application is shown as follows:

![](img/d7c3f2018603810fae83e1a6fd98749e_118_0.png)

### GridLayout

GridLayout is an excellent option for dealing with layouts. It has several configuration fields and it is possible to organize the controls of a composite by associating each one with a layout data object provided by the GridData class.

In GridLayout, it is necessary to specify the number of columns of a grid. The widgets are assigned to the columns in the order they are created, starting from the left and moving to the right. When no more columns are available, the next control is placed in the next row. It provides great flexibility, as each control can also span cells horizontally and vertically.

The following public fields are available for GridLayout:
- horizontalSpacing: Specifies the number of pixels among the horizontal edges of a cell
- makeColumnsEqualWidth: Specifies whether all the columns need to have the same width
- marginBottom: Specifies the number of pixels in the vertical margin of the bottom edge of a cell
- marginHeight: Specifies the number of pixels in the vertical margin of the bottom and top edges of a cell
- marginLeft: Specifies the number of pixels in the horizontal margin of the left edge of a cell
- marginRight: Specifies the number of pixels in the horizontal margin of the right edge of a cell
- marginTop: Specifies the number of pixels in the vertical margin of the top edge of a cell
- `marginWidth`: Specifies the number of pixels in the horizontal margin of the left and right edges of a cell
- `numColumns`: Specifies the number of cell columns in the `Layout` widget
- `verticalSpacing`: Specifies the number of pixels among the vertical edges of a cell

When applying `GridLayout` to position the elements of an application, it is necessary to create a `GridData` object to provide further information on how this positioning will be performed. We show the main public fields that are available for the `GridData` class:
- `verticalAlignment`: Specifies the vertical alignment
- `horizontalAlignment`: Specifies the horizontal alignment
- `widthHint`: Specifies the amount of space in pixels to be used as the default for a cell width
- `heightHint`: Specifies the amount of space in pixels to be used as the default for a cell height
- `horizontalIndent`: Specifies the number of pixels to be placed on the left of the cell
- `horizontalSpan`: Specifies the number of columns that the cell will fill
- `verticalSpan`: Specifies the number of rows that the cell will fill
- `grabExcessHorizontalSpace`: Grabs the horizontal remaining space if it is available
- `grabExcessVerticalSpace`: Grabs the vertical remaining space if it is available

We show an example of how to use `GridLayout` and `GridData` as follows:

```
Display display = new Display ();
Shell shell = new Shell (display);
GridLayout layout = new GridLayout(4, false);
shell.setLayout(layout);
Button b = new Button(shell, SWT.PUSH);
b.setText("LEFT, TOP");
b.setLayoutData(new GridData(SWT.LEFT, SWT.TOP, true, true, 1, 1));
b = new Button(shell, SWT.PUSH);
b.setText("LEFT, CENTER");
b.setLayoutData(new GridData(SWT.LEFT, SWT.CENTER, true, true, 1, 1));
b = new Button(shell, SWT.PUSH);
b.setText("LEFT, BOTTOM");
b.setLayoutData(new GridData(SWT.LEFT, SWT.BOTTOM, true, true, 1, 1));
b = new Button(shell, SWT.PUSH);
```
