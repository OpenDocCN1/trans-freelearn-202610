
# What is Workplace XT?

IBM provides complete, comprehensive APIs for writing applications to work with the CM product and the P8 platform. They also provide several pre-built, ready to use environments for working with CM. These range from connectors and other integrations, to IBM and third-party applications, to standalone applications provided with CM. Business needs will dictate which of these will be used. It is common for a given enterprise to use a mix of custom coding, product integrations, and standalone CM applications. Even in cases where the standalone CM applications are not widely deployed throughout the enterprise, they can still be used for ad hoc exploration or troubleshooting by administrators or power users.

XT is a complete, standalone application included with CM. It's a good application for human-centered document management, where users in various roles actively participate in the creation and management of individual items. XT exposes most CM features, including the marriage of content management and process management (workflow).

![](img/2295191c450a0c86b2d82df6cec04526_186_0.png)

> XT is a thin client web application built with modern user interface technologies so that it has something of a Web 2.0 look and feel. To run XT, open its start page with your web browser. The URL is the server name where XT is installed, the appropriate port number, and the default context of WorkplaceXT. In our installation, that's http://wj c-rhel.example.net:9080/WorkplaceXT. We don't show it here, but for cases where XT is in wider use than our all-in-one development system, it's common to configure things so that it shows up on port 80, the default HTTP port. This can be done by reconfiguring the application server to use those ports directly or by interposing a web server (for example, IBM HTTP Server, IHS) as a relay between the browser clients and the application server. It's also common to configure things such that at least the login page is protected by TLS/SSL. Details for both of these configuration items are covered in depth in the product documentation (they vary by application server type).

For some of the examples in this chapter, we'll log on as the high-privileged user poweruser, and, for others, we'll log on as the low-privileged user unpriv. We mentioned both of those directory users in Chapter 2, Installing Environmental Components. If you didn't create them, you can create them now or substitute any pair of non-administrator accounts from your own directory.

## Browsing folders and documents

Let's have a look at XT's opening screen. Log onto XT as user poweruser. With the folder icon selected from the top-left group of four icons, as in the figure below, XT shows a tree view that allows browsing through folders for content.

![](img/2295191c450a0c86b2d82df6cec04526_187_0.png)

Of course, we don't actually have any content in the Object Store yet, so all we see when we expand the Object Store One node are pseudo-folders (that is, things XT puts into the tree but which are not really folders in the Object Store).

Let's add some content right now. We'll have a lot more to say about the technical features of foldering and containment in Chapter 7, Major CM Features. For now, we'll concentrate on the user view of things.

## Adding folders

In the icon bar are two icons with small, green "+" signs on them (you can see them in the screenshot above). The left icon, which looks like a piece of paper, is for adding documents to the currently expanded folder. The icon to the right of that, which looks like an office supply folder, is for adding a subfolder to the currently expanded folder.

Select Object Store One in the tree view, and click the icon for adding a folder.

![](img/2295191c450a0c86b2d82df6cec04526_187_1.png)

The first panel of a pop-up wizard appears, as shown above, prompting you for a folder name. We have chosen the name ***Literature*** to continue the example that we started in *Chapter 4, Administrative Tools and Tasks*. Click the **Add** button, and the folder will be created and will appear in the tree view. Follow the same procedure to add a subfolder to that called shakespeare. That is, create a folder whose path is /**Literature**/shakespeare.

You can modify the security of most objects by right-clicking and selecting **More Information | Security**. A pop-up panel shows the object's **Access Control List (ACL)**. We'll discuss ACLs in more detail in *Chapter 8, Security Features and Planning*. For now, we just want to allow other users to add items to the shakespeare folder (we'll need that for the illustration of entry templates when we get to that section below). Open that folder's security panel. Click the link for #***AUTHENTICATED-USERS***, and check the **File In Folder** box in the *Allow* column, highlighted in the following screenshot:

![](img/2295191c450a0c86b2d82df6cec04526_188_0.png)

### Adding documents

Now let's add some actual documents to our repository. We'll add a few of Shakespeare's famous works as sample documents.

> There are many sources for electronic copies of Shakespeare's works readily available on the Internet. One of our favorites for exercises like this is at the Massachusetts Institute of Technology: http://shakespeare.mit.edu. It's handy because it's really just the text without a lot of notes, criticisms, and so on. The first thing you see is a list of all the works categorized by type of work, and you're only a click or two away from the full HTML text of the work. It doesn't hurt that they explicitly state that they have placed the HTML versions in the public domain.

We'll use the full versions in a single HTML page for our sample documents. In some convenient place on your desktop machine, download a few of the full text files. We chose *As You Like It* (`asyoulikeit_full.html`), *Henry V* (`henryv_full.html`), *Othello* (`othello_full.html`), and *Venus and Adonis* (`VenusAndAdonis.html`).

Select the `/literature/shakespeare` folder in the tree view, and click the icon for adding a document. The document add wizard pops up, as shown next:

![](img/2295191c450a0c86b2d82df6cec04526_189_0.png)

Browse to the location of the first document file, `asyoulikeit_full.html`, and click the **Next** button. Don't click **Add Now** or you won't get the correct document class for our example.

![](img/2295191c450a0c86b2d82df6cec04526_189_1.png)

Initially, the class **Document** is indicated. Click on **Class** and select **Work of Literature**. The list of properties automatically adjusts to reflect the custom properties defined for our custom class. Supply the values indicated (note in particular that you have to adjust the **Document Title** property because it defaults to the file name). XT uses the usual convention of marking required properties with an asterisk. Click **Add**.

Repeat the above steps for the other three documents. You'll now have a short list in the shakespeare folder.

| Name ▲ | Size | Modified By | Modified On | Major Version |
|---|---|---|---|---|
| As You Like It | 226 KB | poweruser | 10/16/10 1:10 PM | 1 |
| Henry V | 263 KB | poweruser | 10/16/10 1:13 PM | 1 |
| Othello | 298 KB | poweruser | 10/16/10 1:21 PM | 1 |
| Venus and Adonis | 63 KB | poweruser | 10/16/10 1:22 PM | 1 |

XT also provides a "landing zone" for the drag-and-drop of documents. It's located in the upper right-hand corner of the browser window, as shown next. This can save you the trouble of browsing for documents in your filesystem. Even though it can accept multiple documents in a single drag-and-drop, it prompts only for a single set of property values that are applied to all of the documents.

![](img/2295191c450a0c86b2d82df6cec04526_190_0.png)

## Viewing documents

Clicking on a document link in XT will lead to the download of the content and the launching of a suitable application. For most documents, the web browser is used to find and launch an application based on the document content type, although XT does have some configurability in its site preferences for customizing that behavior. The behavior you can normally expect is the same as if you clicked on a link for a document on any typical website.

For graphical image content (JPEG, PNG, and similar formats), XT launches the Image Viewer applet. The Image Viewer applet is especially handy for dealing with Tagged Image Format Files (TIFF) graphics because most browsers do not handle TIFF natively. It is common for fax and scanning applications to generate TIFF images of pages. However, even for common graphics formats that can be rendered by the browser, the Image Viewer applet has more functionality. The most interesting extra features are for adding textual or graphical annotations to the image. Rather than directly manipulating the original image, the annotations are created in an overlay layer and saved as Annotation objects in the repository. For example, in the image below, being displayed in the Image Viewer applet, the stamp tool has been used to mark it as a DRAFT. That annotation can easily be repositioned or even removed without affecting the original image.
