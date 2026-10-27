# What just happened?

With the custom Smarty tag {print}, you use the core module Printing. At the place where you have added the tag, a link Print this page is displayed. When you click on the link, a new page is opened with a print friendly version of the page. It does not have any special styles in it, and it does not display a main template but only the content of the page.

There are some useful parameters that help you to customize the link Print this page and its behavior.

+   - text="Print this page": Overrides the default text for the print link.
+   - popup="false": Set this to true and the page for printing will be opened in a new window. Be careful, some browsers block popup windows!
+   - script="false": Set this to true and the print version will automatically open the print dialog of the browser.
+   - includetemplate="false": If this is set to true, not only the content of the page, but the whole page template including the content of the page, header, navigation, and footer area will be displayed.
+   - showbutton="false": Set this to true to show a graphical button instead of the text Print this page.

If the preceding parameters do not offer enough ways for you to customize the look and feel of the print link and print view, you are free to customize the module in the admin console (Extensions | Printing). There are four tabs:

+   - **Link template**: The template that is used to display the link to the print (or PDF) version of your website.
+   - **Print template**: The template that is used to display the print version. You can add your own HTML markup here, like URL of the website, website company, or contact details, so that this information is automatically displayed in the print version of the website.
+   - **Override print stylesheet**: Gives you the possibility to insert your own, custom CSS to the print version. Normally, you would use it to define the font size and set it in points as often used in print works.
+   - **PDF settings**: We will discuss this tab later in the section *Generating a PDF version of the page.*