
As the number of web pages on websites grew, weekly—or even daily—tasks involved updating up to 50 pages manually. Every time a change was needed in a shared part of a web page (such as the header, footer, or navigation pane), these repeated actions proved to be quite tedious.

Let's investigate some solutions that were devised for this problem.

## Server-side includes

One attempt to remedy this repeated manual work was called a **server-side include**, or **SSI**. This markup element was created to allow web designers to include pieces of pages (for example, the header) without having to repeat content and markup. Then, when the page was generated, the tag would be replaced with the resultant HTML output.

For example, three links on a web page, Home, About Us, and Contact Us, would have the following markup:

```
<a href="home.html">Home</a>
```

```
<a href="about_us.html">About Us</a>
```

```
<a href="contact_us.html">Contact Us</a>
```

This HTML could be placed inside a file called navigation.ssi. The include would be called as follows:

```
<!--#include virtual="navigation.ssi" -->
```

After the web server processed this, the result shown on the page would be the same as that shown in the preceding example. Next, another similar approach was used to allow for dynamic content to be produced.

## The Common Gateway Interface

The **Common Gateway Interface** or **CGI**, allowed programs written in languages such as Perl to be included in a web page, providing added functionality such as a page counter. The actual place in the HTML page that called this code would again be replaced with the resulting HTML output.

A Perl script that counted the number of page visitors was placed into the cgi-bin directory and called as follows:

```
<p>This page has been visited
```

```
<!--#exec cgi="/cgi-bin/counter.pl"--> times.</p>
```

This would produce the following result on the web page:

```
`<p>This page has been visited 5349 times.</p>`
```

The number 5349 was produced by this code and displayed on the page.

## Forms

Another important part of the history of web development was **forms**. Forms allowed a simple web page to transform itself into an actual web application. Web forms replicated the functionality of traditional forms found in desktop applications. Web forms also enabled end users, as opposed to the webmaster, to add content—for example, in forums, submission forms were used.

On the public-facing portion of a website, submissions from these forms added even more content as websites grew quickly in size. Sites soon effectively became software applications, more than just a collection of files with markup. Soon, more than just serving simple pages, web server modules were created to preprocess entire pages as programs.

## Web page preprocessors

Another interesting part of the evolution of modern web development was the ability to use whole page preprocessors, such as PHP. In fact, this language, recursively called **PHP: Hypertext Preprocessor**, explains exactly what it does. These files had a different file extension, and the web server (such as Apache) could process the entire page as an actual program and output the result as HTML tags and content.

Next, let's move into the modern era: **Content Management Systems (CMS).**

## Content Management Systems

Soon, databases such as MySQL were included in affordable web hosting plans so that webmasters could easily use them. Database tables could be queried, and the results would enrich and add meaning to many dynamically generated pages. This laid the groundwork for the first CMS.

By allowing website administrators and *webmasters*, as they were historically called, to manage content in databases, *dashboards* or *control panels* were created. A web-based software application entirely separate from the actual website, these administration tools allowed website content to be easily created, edited, and deleted from the website itself.

Again, this caused websites to grow both in size and complexity very rapidly.

## WordPress

**WordPress** quickly rose as the most widely used CMS. Extending the functionality of WordPress, a collection of plugins was developed whereby anybody could develop and install a plugin. This meant that at any one time, a WordPress website could potentially have hundreds of different plugins installed all at the same time.

Most of these were maintained by ambitious volunteers, and new functionality was quickly added. This phenomenon led to rapid release cycles and many different versions of each plugin in use at the same time.

## Ajax

Another major innovation was the creation of Ajax, or *Asynchronous JavaScript and XML*. Ajax allowed a page to fetch information from a remote source and then, with the result, update that page's content, layout, or style without requiring the page to reload. This *instant update* made websites that still required pages to reload seem cumbersome and slow. Websites again slowly evolved to be one step closer to becoming software applications, similar to desktop applications or single-page applications.

## JavaScript frameworks

As the complexity of this dynamic page manipulation grew, **JavaScript frameworks** were born. These helped to reduce development time and enforce best practices. They also provided JavaScript code that could be functional in as many versions of as many browsers as possible. There are still as many as 100 variants of browsers such as Internet Explorer, Firefox, Opera, and Google Chrome running on desktop computers, tablets, and mobile devices.

## The rise of the cloud

As the web grew in popularity, web hosting costs decreased. The availability of very low-cost web hosting increased rapidly as many companies entered into the market. Website hosting companies and cloud providers saw an opportunity in this, putting the ability to create a website within reach of almost anybody with very little technical skill, at a very low cost.

As history would show, as with the automobile, sometimes the best inventions also foster many problems. Just as the invention and evolution of the automobile has led to crashes, traffic jams, and safety and security issues—so did websites! Since the majority of websites on the internet were created by amateurs and hosted on servers that were not adequately secured, vulnerabilities on the server were soon discovered and then taken advantage of.

Using a combination of outdated or insecure WordPress plugins, insecure JavaScript, and general misconfiguration of web servers, compromised websites could then be used to send spam emails through utilization of the **Simple Mail Transfer Protocol (SMTP)**. These exploiters inserted email addresses and email contents into database tables and then, in an automated manner, sent thousands of emails per hour, hopefully convincing people to spend money. Keeping a server secure by limiting which tools on the server could be accessed by a user, locking down directories, not allowing repeated login attempts, and other techniques were used to reduce security risk.

However, the overhead of having to make sure that all of a website's plugins were up to date soon became overwhelming. Situations such as version 3.12.26 of a particular plugin not working with version 6.8.14 of another became commonplace. The web slowly became the Wild West!

## High-speed access

As high-speed internet became pervasive, connection speeds increased to approach 50-100 **megabits per second (Mbps)** on home computers, through **digital subscriber line (DSL)** and cable internet. Web designers took more liberty regarding the amount of information, images, media files, JavaScript, and anything else that was to be placed on a single web page. This led to a slowing down of what were previously quick-loading websites.

Also, the pages required multiple steps to actually be dynamically generated. Live-running code and database queries needed to be interpreted on each and every request. Page time slowed down because of all of the steps needed to deliver a single web page. As history has shown, a new solution will always arise to address new problems. This solution is called the **Jamstack**.

Let's begin to dissect what it is.

## The rise of the Jamstack

The Jamstack represents all that was good about the initial days of web design, together with all of the things that are good about modern web development. Static site generation, as it's sometimes referred to, means simply generating a website comprised of pages that are literally files and nothing more. There is no backend preprocessor and no database, and all of the server complexity is abstracted away.
