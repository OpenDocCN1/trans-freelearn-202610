
New terms and important words are introduced in a bold-type font. Words that you see on the screen, in menus or dialog boxes for example, appear in our text like this: "Now, click on the edit permissions link to set up the permissions for our moderator role".

Warnings or important notes appear in a box like this.

Tips and tricks appear like this.

## Reader feedback

Feedback from our readers is always welcome. Let us know what you think about this book, what you liked or may have disliked. Reader feedback is important for us to develop titles that you really get the most out of.

To send us general feedback, simply send an email to feedback@packtpub.com, making sure that you mention the book title in the subject of your message.

If there is a book that you need and would like to see us publish, please send us a note via the SUGGEST A TITLE form on www.packtpub.com, or send an email to suggest@packtpub.com.

If there is a topic that you have expertise in and you are interested in either writing or contributing to a book on, see our author guide on www.packtpub.com/authors.

## Customer support

Now that you are the proud owner of a Packt book, we have a number of things to help you to get the most from your purchase.

## Errata

Although we have taken every care to ensure the accuracy of our contents, mistakes do happen. If you find a mistake in one of our books — maybe a mistake in text or code — we would be grateful if you would report this to us. By doing this you can save other readers from frustration, and help to improve subsequent versions of this book. If you find any errata, please report them by visiting http://www.packtpub.com/support, selecting your book, clicking on the let us know link, and entering the details of your errata. Once your errata are verified, your submission will be accepted and the errata added to any list of existing errata. Any existing errata can be viewed by selecting your title from http://www.packtpub.com/support.

## Piracy

Piracy of copyright material on the Internet is an ongoing problem across all media. At Packt, we take the protection of our copyright and licenses very seriously. If you come across any illegal copies of our works in any form on the Internet, please provide the location address or website name immediately so that we can pursue a remedy.

Please contact us at copyright@packtpub.com with a link to the suspected pirated material.

We appreciate your help in protecting our authors, and supporting our ability to bring you valuable content.

## Questions

You can contact us at questions@packtpub.com if you are having a problem with some aspect of the book, and we will do our best to address it.

## 1 Introducing Good Eatin'

Although several books have been written about Drupal, most have been written for developers to help them understand how to build sites with Drupal, enhance sites by using custom modules, and change the appearance of Drupal based sites by using themes. In this book, we take a different approach. Throughout the book, we will develop a web site for a (fictional) business, to meet the needs of this business.

We have included an explanation of all of the key features in Drupal that most small-to-medium sized businesses would want to use on their sites. A thorough step-by-step description of how to integrate each piece of functionality into your web site is included, along with an explanation of the business need that is being satisfied by using the functionality.

After reading this book, you will be able to adapt the techniques used in this book to either develop a site for your company on your own, or work with a development firm to create a web site that truly meets the needs of your business.

In this chapter, we will introduce our client, the Good Eatin' Bistro, and discuss their plans to build a web site. We will also introduce Drupal and describe why it is a good fit for our client's web site.

### The Good Eatin' Bistro

Chef Wanyama is the owner of the Good Eatin' Bistro, a popular upscale restaurant. The past few years have been very good, and he has expanded from one restaurant to three. Each restaurant is decorated in a safari theme and Chef Wanyama is extremely proud of the decor. The Good Eatin' Bistro serves a wide variety of food, but specializes in creating meals using exotic game meats.

Chef Wanyama has recently added both take out and delivery to his menu and would like to do additional promotion for this aspect of his business, which he feels will bring in additional revenue. He also gives cooking classes, and would also like to promote these.

### Plans for a Good Eatin' web site

Chef Wanyama had a web site designed for him several years ago, but it has proven to be difficult to maintain, and is therefore several years out of date. Chef Wanyama would like to build a new web site for his businesses to highlight the new specials at his restaurant. He would like to provide a destination for his customers where they can visit his restaurant online, learn more about the restaurant, find out about specials, and receive discounts if they are loyal customers.

Chef Wanyama has several key goals for his site:

- 1. The site must be easy to update so that it is always current.
- 2. Chef Wanyama should be able to make updates to the site without having to pay a consultant to do so.
- 3. He should be able to delegate simple tasks without compromising on site security.
- 4. The overall site should be secure and easy to maintain.
- 5. It should be possible to add new functionality to the web site without requiring a complete start over.

After Chef Wanyama came to us expressing his desire to have a new web site built, we sat down with him to determine what pages and sections should be added to the web site, so that we can plan our development. Based on these discussions, we decided on the following structure for the site:

- 1. Home Page: This includes links to other areas in the site, current events, and sample menu items. Chef Wanyama may want the home page to have a look different from the rest of the site.
- 2. Menu Pages: This includes food, wine, and cocktails. There will be two methods for creating menu pages: a simple method where each page is developed independently, and a second method where editors can fill in a template for each menu. Each menu item will be categorized, so that customers can search the menu more easily.
- 3. Menu Search: This allows visitors to search the menu for foods, based on a variety of criteria.
- 4. Menu item rating: This allows visitors to rate items on a scale of 1-5.
- 5. About Us Page: This page provides contact information.
- 6. Guest Book: This allows visitors to comment on the restaurant and give feedback. Chef Wanyama can see when new comments are added, and he can optionally remove comments if they do not meet the site's standards.
- 7. **Map**: This helps the visitor find all of the Good Eatin' restaurants, using Google Maps.
- 8. **Polls and Surveys**: From time to time, the restaurant will have polls to see what items patrons want added to the menu, and what they think of the new items on the menu.
- 9. **Picture Gallery**: This shows the pictures of the food on the menu as well as pictures of the restaurants.
- 10. **Monthly Newsletter**: This is a newsletter that visitors can subscribe to. Chef Wanyama will be able to create the newsletter content on the site. Customers can also view the previous editions of the newsletter.
- 11. **Event Calendar**: This showcases musical events and other events occurring at the Good Eatin' restaurants.
- 12. A **Member Rewards site**: This gives details of the free meals for members who have visited the restaurant a specified number of times.
- 13. **Online ordering**: This will allow visitors to order take out and delivery.
- 14. **Forum**: This is a forum to solicit suggestions and ideas for improvement, to discuss policies, and so on.
- 15. **Blog**: This will be used by Chef Wanyama and his employees for discussing topics important to them, including information on new menu recipes, running a restaurant, and more.
- 16. **Administration Area**: This helps Chef Wanyama optimize his site so that the visitors can make the most out of the web site.

### Selecting a foundation for the Good Eatin' site

In this section, we will explore a range of possible technologies that could be used to build the Good Eatin' site. The pros and cons of each method will be discussed, and we will choose the final technology that will be used to build the site.

### Simple static web pages

Most web sites are created by simply creating standalone HTML pages that do not change. These pages can be created with a dedicated web site design program or with a simple text editor such as Microsoft Notepad. These pages are then uploaded to the web server using an FTP client, or another transfer client provided by the web host.
