By now our solution works as we planned — we have a roll-over effect for thumbnails of images that have the Lightbox effect active (image gallery), and controls on the contact form are distinguished with a yellow background while they are focused. We have modified the original Artisteer-created `img.art-Lightbox` class and added a pseudo class for active inputs.

8. If you edit the `style.css` file in an exported template, you will see that our code is surrounded with proper comments showing the beginning and end of our modifications.

While writing code in the **Additional CSS styles:** field is not too comfortable, you may wonder why you should put your code here instead of editing the exported files' code directly, or using your own, additional CSS file. I agree that writing code with any code editor is much more convenient for sure, but you should remember one advantage of using the technique described in this example, and doing it directly in Artisteer also has some advantages. If you put your code directly into Artisteer, your code is stored with the `.artx` project file, which means you don't have to remember about your changes and rewrite them after each export, which is convenient for future changes. That's why I think that if the changes are not too big and you don't have to touch the rest of the exported code, this could be quite a good method.

# Another way of modifying CSS files

This tip applies to all kind of templates. An example would be the `OneDayBlue.artx` file.

In the *Styling from scratch* and *Adding additional CSS to the project* tips, we have modified the CSS stylesheets of the relevant project. Each of these examples presents a different approach to the problem:

+   - We modified an exported CSS template file
+   - We added some CSS definitions directly in Artisteer

Each of these techniques has its advantages and disadvantages. If you edit the exported CSS file, you can do it with your favorite text editor, which is probably equipped with many tools that make the job more comfortable (like code highlighting). Also, while editing the exported CSS file, you will see all of the content (CSS definitions used in the template) and don't have to write blindfolded. The disadvantage of this approach is that if you modify your project in Artisteer (using GUI) and export the new version once again, your file will be overwritten with the new one, and your modifications will be lost. You will have to rewrite your changes.