
## 3 CMS Templates

Until this moment, we have focused on designing static websites (it's called a Website Template in Artisteer). The term **static website** can be a little bit embarrassing for you, because it's naturally considered with a "page on which nothing moves". You may think that if you add, for example, a slideshow to a static website, it will cease to be static. This is a very natural and logical way of thinking, but in this case, the commonly used terminology is quite different. By saying static website, we don't think about its content, but about the way the page is generated.

## What is CMS

In a static HTML website, the content is hardcoded – it is a part of the code of the page. Such a website consists of numerous HTML files and all of them are written separately, one-by-one. They could be prepared with the use of various tools (more or less simplifying the job), but each file contains both the look definitions and the content. All those files are copied to the web server, which makes them available for the visitors. This is the most natural and easy-to-understand kind of website.

A dynamic page is definitely more technically advanced. It doesn't exist as a file, but is generated on-the-fly by the web server. After getting the signal that a particular page is to be displayed, a special kind of software gets the proper content (usually from database) and puts it into an available template. After content and template have been successfully combined, the page is ready and is sent to the browser.

**Dynamic website** is thus, not a collection of static pages (HTML files), but a software installed on the web server for managing content, templates, and merging them into dynamic pages.

Dynamic websites work slower than their static equivalents because each page must be generated before being displayed. They have more server requirements, because the server not only displays, but also generates the pages.

So what are the advantages? Well, this is for sure a much more universal solution, and "once the software, known as a content management system (CMS), is prepared, it can be used many times, for various websites. Let's recall our definition of a template. I have written that a template is a collection of common, fixed elements of a website, like the background, header, and footer.

Those elements are common for the entire website. What differentiates individual pages is the content. With the use of a CMS, the content is separated from the layout and stored in an external store (usually in the database). Right before the page is displayed, CMS gets the appropriate content and puts it into a template.

![](img/7f889df2ffdb68108cff34d798a2b9a3_92_0.png)

While the page is displayed, CMS connects fixed elements of the page (template) with proper content from the database

Because the fixed elements are defined only once (they are not duplicated on every page, but exist in only one instance of the template), it means saving of data. But what's even more important is that the separation of look and content allows you to change the template at any time, without any influence to the content. You can change the look of the site at any time and leave the content untouched. If you administer a website or blog over a long period of time, you will surely appreciate such a possibility. The technical separation of appearance and content doesn't mean that you have to enter the content separately, without seeing the template at all (that would be very hard). The more advanced CMS solutions enable you to enter the content online, directly into the browser, with a WYSIWYG editor.

Knowing all the information mentioned earlier, we can form our definition of a CMS.

![](img/7f889df2ffdb68108cff34d798a2b9a3_93_0.png)

> CMS is a universal software for managing websites. It separates the look of the site from its content by using templates and allows the administrator to enter the content directly into the browser. Usually, CMS systems allow you to manipulate the menu structure, too.

There are various types of CMS available in the market. Some of them are commercial, while others are free, and the possibilities of the most well-known solutions go far beyond our definition. Artisteer supports creating templates for several popular CMSs.

Because content editing has a place directly in the browser, there's no need to upload new pages to the server. It really speeds up managing a website as well as adding new content and pages.

Using CMS system has another, maybe the most important advantage, which is the time needed to add new content to the site.

Imagine that you have a website consisting of 50 pages. (You may think that it's a lot by now, but it's not. After you start to develop your site seriously, you will exceed this number quite easily.) Imagine that it's a static website, consisting of separate HTML documents. The common elements are the header, a vertical menu on the left, and the footer. You want to add just one page and want it to be accessible from the menu. The first step is of course preparing the new page. But after that, you will realize that this was a less time-consuming task (if you use any WYSIWYG editor, you can display the template in it and enter the content very fast). The real problem is with modifying the menu. It's not enough to modify the menu on the page just added, but you will have to modify the menu on all the other pages! (The new page should be accessible from any of the existing pages.) Because the menu is hardcoded on every page (it's just a fragment of their source code), and a static website is just a collection of separate HTML documents, this means you have to edit and modify all the existing files! In our case, adding one page requires you to modify 50 existing files, and don't forget that you have to also upload all of them onto the server, after your work is finished.

It would be the same situation not only with the menu, but in the case of any fixed element. Do you want to change the phone number in the footer? You have to modify the footer on all pages.

But if you use a CMS, adding a new page is achieved with a few mouse clicks, and the menu adapts automatically to the new site's structure. With the footer, it would be a similar process, and we would need to change it only once. Ease of modification and time savings is what makes CMS the best option for bigger sites.

![](img/7f889df2ffdb68108cff34d798a2b9a3_94_0.png)

Static website: Adding a single page takes more time and work, the more pages the site consists of

With a CMS, adding a single page takes less amount of time and work, no matter how big the site is.

Based on the foregoing, the following conclusion follows:

Adding a single page to a static website takes more time and work, the more pages the website consists of. Adding a single page using a CMS always takes a fixed amount of time and work, no matter how big the entire website is.

But wait! In the previous chapter, we could clearly observe that after adding a page to the project, the menu gets adapted automatically too. Same is the case with the footer or header (fixed elements of layout).

It's true that Artisteer acts similar to a CMS, and this is one of its most important advantages. But note that even when using Artisteer, after changes are done, you will still have to upload all the modified files to the server (and replace the old ones).

Can Artisteer be considered as an alternative for CMS? Well, for small websites, yes. You can use Artisteer features both for designing the template and entering the content, having all together in one tool. But administration of bigger sites will be easier with the use of CMS because you won't have to upload all the modified files onto the server. There are also things that cannot be done in Artisteer alone (for example, sending a message using a Contact form). Does this mean that CMS is better than Artisteer? Not in the least, it's just a different kind of tool. CMS is a software for managing websites, while Artisteer is a software for designing templates.
