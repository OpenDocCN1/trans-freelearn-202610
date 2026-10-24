
## Basic formatting

These tags allow you to change the appearance of the text when it is displayed on your web site. To use one of these tags, simply place the starting tag before the text whose style you want to change, and the ending tag when you want to return to the previous style. For example, to set the text hello in bold, you would enter <strong>hello</strong>. You can nest tags to apply more than one style to a piece of text. For example, to bold and italicize the text hello, you enter <em><strong>hello</strong></em>. When you apply more than one tag to a piece of text, make sure the pairs of open and close tags are correctly nested, and do not overlap.

| Start Tag | End Tag | Purpose |
|---|---|---|
| <em> | </em> | Provide emphasis to a piece of text; this is done by italicizing the text. You should use this tag when you want to draw the reader's attention to a particular piece of text. |
| <strong> | </strong> | Make a piece of text stand out from the rest of the text; this is done by bolding the text when it is displayed in a browser. Setting text in bold can help readers to quickly scan your page to determine if they want to read it in more detail. You should use this tag when a piece of text is more important than the remainder of the text. |
| <cite> | </cite> | This tag is used to indicate a citation from another source; this is typically rendered as italicized text. |
| <code> | </code> | The code tag displays the text using fixed-width characters, which allows characters to line up. This greatly aids readability for text where the position of characters on subsequent lines is important. |

## Creating lists

There are three styles of list that you can create using HTML tags: ordered lists, unordered lists, and definition lists. The ordered list, precedes each element in the list with a number. The unordered list precedes each element in the list with a bullet. The definition list first displays the definition term and then indents the definition under the term. To create a list, put the list start tag at the beginning of the list, the list end tag at the end of the list, and surround each item in the list with list item tags.

| List Type | Start Tag | End Tag | Element Tag | Example Code | Example List |
|---|---|---|---|---|---|
| Unordered List | <ul> | </ul> | <li>element</li> | <ul>
<li>Item 1</li>
<li>Item 2</li>
<li>Item 3</li>
</ul> | • Item 1
• Item 2
• Item 3 |
| Ordered List | <ol> | </ol> | <li>element</li> | <ol>
<li>Item 1</li>
<li>Item 2</li>
<li>Item 3</li>
</ol> | 1. Item 1
2. Item 2
3. Item 3 |
| Definition List | <dl> | </dl> | <dt>term name</dt>
<dd>description</dd> | <dl>
<dt>Address</dt>
<dd>2500 Main St.
Anytown, CO
USA</dd>
<dt>Phone</dt>
<dd>303-555-5555</dd>
</dl> | Address
2500 Main St.
Anytown, CO
USA
Phone
303-555-5555 |

## Linking to other pages

You can use the <a> tag to create a link to another part of your site or to another web site. This tag is slightly more complicated than the other tags because you must specify the destination that you would like the user to be taken to when the link is clicked.

The link tag has the following format: `<a href="destination">Link Text</a>`. For example, to link to the Good Eatin' home page, we would use the following tag:

```
```html
<a href="http://goodeatin.drupalbyexample.com">Good Eatin' Bistro</a>
```
```

## Editing a page

After you have created a page, you can change the text on the page at any time by editing the page. To edit a page, first open the page in your browser, and then select the Edit link that appears next to the title of the page.

![](img/b332d58f89551dfc3ae9a7cf47e6a08f_43_0.png)

Drupal displays a form that is similar to the form we used when adding the page to the site.

## Security considerations

You can control who is allowed to add pages to your web site by using permissions. By default, only the administrator is allowed to create pages and stories. If you are working on the site with a team, it is a good idea to create new roles which give team members access to only the functionality that they need to do their jobs.

Let's create an editor role so that other people in our team can add pages to our site. To create a new role, click on **User Management** and then **Roles**, within the **Administer** menu. Type **editor** as the name of the role and click the **Add role** button. Now that we have a new role, we can assign permissions to it by selecting the **edit permissions** link. The permissions you set will depend on what the user needs to do to perform his or her job and how much you trust the user. In most cases, it is best to give users fewer permissions in the beginning, and then give them additional privileges as they become more familiar with the site.

For our site, we will give the editor the following permissions, which are located below the **node module** heading:

- **Create page content:** Allows the user to create pages for the site
- **Edit own page content:** Allows the user to edit pages they have created, but not pages created by other users

A full list of permissions for creating and editing content is shown here:

| Permission | anonymous user | authenticated user |
|---|---|---|
| **node module** |  |  |
| access content | ✓ | ✓ |
| administer content types | ☐ | ☐ |
| administer nodes | ☐ | ☐ |
| create event content | ☐ | ☐ |
| delete any event content | ☐ | ☐ |
| delete own event content | ☐ | ☐ |
| delete revisions | ☐ | ☐ |
| edit any event content | ☐ | ☐ |
| edit own event content | ☐ | ☐ |
| revert revisions | ☐ | ☐ |
| view revisions | ☐ | ☐ |

If you trust the users who will become editors, you could also give them the privilege to edit any page content, which would give them the ability to edit pages created by other users. If you are using Drupal 5, be careful when deciding which pages a user can edit because allowing a user to edit a page will also allow them to delete it.

If you choose to use other content types such as the **Story** type, you will need to assign permissions to these as well.

We will discuss additional permissions in future sections, so don't worry if you don't know what they all mean now.

## Creating Clean URLs

Due to technical issues with how Drupal creates and displays pages, the default path for a page looks like this:

```
http://www.yoursite.com?q=node/1
```

This format can be confusing to visitors because it is more difficult to type and because it is not as familiar. Some search engines will also not index anything that comes after a question mark in a URL. This effectively makes your site have only one page in the eyes of some search engines. You can convert these to standard URLs by using Clean URLs. This will make the same page's path as follows:

```
http://www.yoursite.com/node/1
```

In Drupal 6 and later versions, you can set up Clean URLs during the installation process. If you are still using Drupal 5 or if you did not configure Clean URLs during installation, you can access this setting by clicking on Site Configuration and then selecting Clean URLs from the Administer menu.

To enable Clean URLs, your site must first pass an automated test. In Drupal 6 and later versions, this is done during installation and when the Clean URLs configuration page is loaded. In Drupal 5, you must click on the Run the clean URL test link on the Clean URLs test page. If your setup meets the requirements, you can click on the enabled link to correct the URLs.

If your setup does not meet the requirements, there is a wealth of information on the Drupal site at: http://drupal.org/node/15365 that will help you determine why, and correct any issues. If you continue to have trouble setting up Clean URLs, you may need to contact your site host.

## Street presence, setting up the home page

In this section, we will create the home page for the Good Eatin' web site. We will cover two possible styles of home pages. The first displays short snippets of text called teasers from other portions of the web site. This type of page is typically used on news sites and blog sites. The other type of home page is a more static page which contains its own unique content. This type of page is typical of most web sites.

## Blog style home pages

The first page that most visitors to your site will see is the home page. Drupal makes it very easy to control the appearance and content of the home page.

By default, Drupal uses a blog style format, in which a portion of several other pages are displayed on the home page. Drupal calls the part of each page that is displayed a teaser.

The teaser is automatically created by Drupal according to the Post Settings, which can be accessed from the Administer menu by clicking first on Content management and then on Post Settings. From this page, you can easily control how many posts will be displayed on the home page, and also how long the teasers will be.
