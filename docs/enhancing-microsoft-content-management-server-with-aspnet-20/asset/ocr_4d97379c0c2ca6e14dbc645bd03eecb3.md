ASP.NET 2.0 introduces the PostBackUrl property to the Button, LinkButton, and ImageButton controls. With the PostBackUrl property, developers may now send postback data to any web form within the site. This feature is particularly useful when working with multiple web forms that share data or interact with one another.

In this tip, we will take a look at the PostBackUrl feature as well as an essential workaround required for it to work correctly on MCMS sites. Here’s what we will be building. We will start by creating a page that accepts input from a user. Next, we will add a button that has its PostBackUrl property set to another page. Following which, we will build the page specified in the PostBackUrl property of the button and program it to accept the inputs from the first page. Finally, we will take a look at some required workarounds for PostBackUrl properties to work correctly in MCMS sites.

# Passing Data between Pages

A common scenario where data is passed from page to page is to have a search input field on all your postings or a dedicated search page that contains fields for entering keywords and other criteria. The entered search criteria could be submitted to a separate page that performs the search and displays the results.

Or you may have a ubiquitous wizard-style dialog similar to the ones found in popular online stores like Amazon or eBay. Wizards break down a task (say, checking out the items in a shopping basket) into a series of individual steps. Information such as the list of items in the shopping basket, the user’s name, shipping address, and other details are passed from page to page until the transaction is finally committed.

One way of tackling the problem would be to place all sections of the wizard in a single web form. The sections are usually managed by panels or tables. As the user progresses through the task, he or she sees only a portion of the page. Sections not related to the step he or she is working on are hidden from view. In this way, the user never leaves the page. However, the main drawback to this approach is that as the sections increase in both number and complexity, the page becomes progressively more difficult to maintain.

Alternatively, you could call Server.Transfer(), which accepts an input parameter that indicates whether the state and values input fields should be preserved when directing from one page to another. For example, in Page1.aspx, we could call Page2.aspx as follows:

```
```
Server.Transfer("Page2.aspx", true);
```
```

By setting the second, Boolean parameter to true, the state of all form values will be passed to Page2.aspx. From Page2.aspx, we can retrieve input field values using the Request.Form collection:

```
```
string myText = Request.Form["MyText"];
```
```