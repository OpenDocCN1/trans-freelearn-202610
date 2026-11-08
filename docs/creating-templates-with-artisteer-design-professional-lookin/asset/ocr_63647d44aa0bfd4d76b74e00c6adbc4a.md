3. Recall the standard classes we worked with in the *Styling from scratch* tip. We worked with the `art-block`, `art-nostyle`, and `art-article` classes. The first part of its name is the prefix you type in this field. Let's assume that I would write `jakub-` here (from my name). In such a case, instead of the `art-block`, `art-nostyle`, or `art-article` class, Artisteer would generate the class names, namely, `jakub-block`, `jakub-nostyle`, or `jakub-article`, respectively. Changing the default CSS class prefix doesn't change anything in the code, but class names. So why would you do that? There are two reasons:

+   - Sometimes you may want to hide the fact that you prepare your projects with Artisteer. There are people who think that the Artisteer-generated templates are less expensive than the templates made manually or with other tools. Whether they are right or wrong is beyond the scope of this book. If you need, you can change this prefix to anything else.
+   - You may have an external script operating on specified class names. In such a case, it can be faster to adapt the names in your template code than in the script.

4. The **Additional CSS styles:** field below the **CSS Prefix:** field is a text area, where you can put any additional CSS code you want. This code will be added at the end of the `style.css` file. Unfortunately, there's no support for code writing, like coloring the syntax or at least changing the font, so it's a good idea to prepare your code in an external editor and then paste it here.

5. OK, let's put some code in the **CSS Prefix:** field to realize our task. We want to distinguish images when there is a mouse cursor over them, and controls from the contact from when they are active. Write the following code into this field:

```
/* hover pseudo-class for images
   We use opacity filter */
img:hover
{
    opacity : 0.4;
    filter: alpha(opacity=40);
}

/* and for inputs while they are focused.
   We set their background color as yellow */
input:focus
{
    background-color: yellow;
}
```