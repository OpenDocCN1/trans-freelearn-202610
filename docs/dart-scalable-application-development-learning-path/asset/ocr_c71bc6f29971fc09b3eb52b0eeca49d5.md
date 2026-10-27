# The About dialog box

The standard About box for an application contains a small amount of information and a link to a website. This dialog is visually identical to the Alert box apart from the addition of the hyperlink, and as this involves changing the dialog interface elements, this will be implemented in a new class called AboutDialog:

![](bbox=[0.241, 0.247, 0.758, 0.511])

This is created from the showAbout method of the TextEditor object:

```
void showAbout(MouseEvent event) {
    AboutDialog textEditorAbout = new AboutDialog(AppTitle,
    "TextEditor for the Web", "http://www.packtpub.com", "Homepage",
    300, 200);
    textEditorAbout.show();
}
```

The AboutDialog box inherits from the Dialog class and calls the constructor of the Dialog class via super, which calls the constructor to form the foundation of the object:

```
AboutDialog(String titleText, String bodyText, this.linkUrl, this.
linkText, int width, int height)
: super(titleText, bodyText, width, height, true, false){
```