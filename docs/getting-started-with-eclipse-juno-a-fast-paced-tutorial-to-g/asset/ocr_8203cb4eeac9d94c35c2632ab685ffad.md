# FontDialog

The FontDialog can be used for selecting fonts for a text widget in a graphical user interface. Its use is similar to the aforementioned examples.

A snippet of code along with the corresponding output is shown as follows:

```
...
Text text = new Text(shell, SWT.CENTER | SWT.BORDER);
FontDialog fd = new FontDialog(shell, SWT.NONE);
fd.setText("Font Selection");
fd.setRGB(new RGB(0,0,255));
FontData defaultFont = new FontData("Verdana",10,SWT.BOLD);
fd.setFontData(defaultFont);
FontData newFont = fd.open();
text.setFont(new Font(display, newFont));
text.setForeground(new Color(display, fd.getRGB()));
...
```

The corresponding output is as follows:

![](bbox=[0.274, 0.45, 0.726, 0.74])