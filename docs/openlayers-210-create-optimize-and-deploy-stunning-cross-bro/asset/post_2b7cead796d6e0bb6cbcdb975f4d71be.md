
## 2

## Squashing Bugs With Firebug

OpenLayers is, at a fundamental level, not doing anything that is conceptually too hard to grasp. It gets map images from a server, and puts them together. From a technical level, however, there is a lot of work going on, and it might seem magical how it all works together so well.

Fortunately, there are many tools to dispel any potential magical thinking we might have and show us how OpenLayers is working behind the scenes. Firebug, a free and open source plugin for Firefox, is one such great tool. Speeding up development time, viewing network communication, and squashing bugs are just a few things that Firebug, and other web development tools, do that make them hard to live without.

To really use OpenLayers effectively and to its full potential, we need to understand how it works. In this chapter, we'll try our best to do just that, by using web development tools to examine OpenLayers' inner workings. By doing so, we'll accomplish two things. First, we'll become familiar with these tools which will significantly help us when developing our maps. Secondly, and more importantly for now, we'll gain a better understanding of how OpenLayers works.

Throughout this chapter we'll cover:

- What Firebug and other development tools are
- Setting up Firebug
- Each of the Firebug Panels
- Using the JavaScript Command Line Console panel

### What is Firebug?

**Firebug** is a free, open source addon for Firefox. If you do not have Firefox, I recommend downloading it (it is also free and open source). However, other modern and standards based browsers, such as Google's Chrome, Apple's Safari, and Opera, also work well and have great built in developer tools.

Firebug, and other web development tools, makes the web development process much easier and quicker. What do I mean by this? With these tools, we can change anything on our site, on the fly, without editing or saving any files. We can type in JavaScript code with a command line interface and execute it immediately. We can view all the requests that our web page sends to servers, along with the server's reply. For example, if our map isn't able to get back map images from the server, we could examine the requests our page is making and find out if we have any typos or haven't set up our map layer properly.

Using these tools makes it a lot easier to develop not only an OpenLayers mapping application, but any web application, and makes it easier to fix any bugs we encounter in the process. We'll focus on Firebug in this chapter and refer back to it throughout the book, but other tools such as Google's Chrome and Apple's Safari's built in developer tools work just as well (although some functionality may vary).

### Setting up Firebug

Since Firebug is an extension of Firefox, you'll need to first install Firefox. You can download it for free at http://getfirefox.com. After that, Firebug can be freely downloaded at http://getfirebug.com. When you click on the link to download Firebug, Firefox will prompt you with a message asking if you wish to install the plugin. After installing, all you have to do is restart Firefox and you'll be good to go.

### Time for Action – downloading Firebug

If you do not already have an up to date version of Firefox installed, please do so now. After you have installed Firefox, set up Firebug by following these steps:

- 1. Go to http://getfirebug.com.
- 2. Click on the **Install Firebug for Firefox** button. Once you click this, you should see a message similar to this:

![](img/1b8cd151c9f278555f9cf255b20d244e_51_0.png)

- 3. Click **Install**, wait for it to finish installing, and then restart Firefox.
- 4. Now that Firebug is installed, you should see a Firebug icon on the bottom right side of your screen.

![](img/1b8cd151c9f278555f9cf255b20d244e_51_1.png)

### What Just Happened?

When Firebug is not enabled, the Firebug Icon is gray. When it is enabled, it has an orange color—this is just a quick way for you to tell if Firebug is enabled or not for the current page you're on.

### Squashing Bugs With Firebug

When you click on the Firebug icon (near the bottom right of your browser's window), Firebug will open and you can start using it. But before we start, let's take a look at what Firebug looks like after initially installing it and clicking on the Firebug icon:

![](img/1b8cd151c9f278555f9cf255b20d244e_52_0.png)

The top row contains two icons (a Firebug icon and a Page Inspector icon), multiple panels that provide specific functionality (they look and act similar to tabs, but the technical term is "panels"), a search box, and finally minimize, maximize, and close the buttons. Let's go over the items, left to right, one at a time.

> The position of the icons may change over time as Firebug is updated; but the general functionality should remain (more or less) the same.

### Firebug controls

Firebug icon: The Firebug icon on the top left contains various commands and options related to Firebug when you click on it.

Page Inspector icon: This icon, a cursor inside a rectangle, is the HTML Inspector. When you click on it, your mouse cursor will identify HTML elements on the web page. So, when you mouse over anything on a website, the element will be outlined in blue and the HTML panel will open up and show you the element your mouse is over.

### Panels

The next set of controls is called panels; each panel provides a different type of function. The panels act like tabs (the two terms can be used interchangeably), but Firebug refers to them as 'panels' in the documentation. Let's go over each panel, since they are, essentially, what makes up Firebug.

### Console panel

Firebug's **Console panel** is where we'll spend most of our time. It acts as a powerful JavaScript command line, or interpreter, which means we can type in JavaScript code and execute it right away—no need to save or edit any files.

One thing that makes this so useful is that we can interact directly with the **DOM (Document Object Model)**—any HTML element on the webpage, including any existing JavaScript code the page contains. So, this means we're able to interact with our OpenLayers map on the fly, issuing commands and testing code to instantly see what works and what doesn't. As you can imagine, this saves a ton of time!

By default, the Console panel is disabled. To enable it, click on the arrow near the **Console** text and select **Enabled.**

![](img/1b8cd151c9f278555f9cf255b20d244e_53_0.png)

### HTML panel

The HTML panel provides not just a display of the HTML source code, but also the ability to quickly edit any HTML element and its associated style. You can add and remove HTML elements, edit HTML attributes, and change nearly anything about the page without having to save any files. It's great for development.

### How it works

Firebug automatically builds a tree structure from your HTML code, allowing you to expand and hide each HTML tag. It is important to note that the code you see in the HTML panel is generated HTML code—the code in the panel may not be exactly the same as the page's source code.

### HTML panel contents

Here is what the HTML tab looks like when Firebug is opened while viewing a webpage:

![](img/1b8cd151c9f278555f9cf255b20d244e_54_0.png)

On the left side, Firebug shows us the HTML of the page. We can right click on any tag and do various things—such as copying the HTML to the clipboard, deleting the element, changing the tag attributes, and more.

On the right side, we see the associated style information for the element we have selected. Here, we can modify or add properties and they will instantly appear on the page. In this example we selected a div element with a class of 'left_sidebar_item'. Looking at the CSS on the right side, there is no definition for the 'left_sidebar_item' class (if there was, we would see something called .left_sidebar_item, with a period in front of it to indicate that it is indeed a class). We do, however, see a definition for #left_sidebar, which is a parent div of the currently selected div.

> If you are unfamiliar with HTML or CSS, the w3schools site is a great resource. For more information on HTML, visit http://www.w3schools.com/html/default.asp and for CSS visit http://www.w3schools.com/css/default.asp.

What does this mean? Well, Firebug lists all inherited style information, and parent element styles propagate down to all their child elements (each child has all its parent's styles, unless the child overrides a style, which doesn't happen in this example). That's why we also see the body, html definition, and every div will display that, since every div sits inside the <html> and <body> tags of the web page.
