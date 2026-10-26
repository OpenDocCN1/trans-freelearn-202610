
![](img/b5ca7252918ddb25344f928f46a6669d_0_0.png)

# Thinking in HTML

Learn to write intuitive HTML and build your sites
on solid foundations

Aravind Shenoy

[PACKT]
PUBLISHING

# Thinking in HTML

Copyright © 2014 Packt Publishing

All rights reserved. No part of this book may be reproduced, stored in a retrieval system, or transmitted in any form or by any means, without the prior written permission of the publisher, except in the case of brief quotations embedded in critical articles or reviews.

Every effort has been made in the preparation of this book to ensure the accuracy of the information presented. However, the information contained in this book is sold without warranty, either express or implied. Neither the author, nor Packt Publishing, and its dealers and distributors will be held liable for any damages caused or alleged to be caused directly or indirectly by this book.

Packt Publishing has endeavored to provide trademark information about all of the companies and products mentioned in this book by the appropriate use of capitals. However, Packt Publishing cannot guarantee the accuracy of this information.

First published: September 2014

Production reference: 1120914

Published by Packt Publishing Ltd.
Livery Place
35 Livery Street
Birmingham B3 2PB, UK.

www.packtpub.com

## About the Author

**Aravind Shenoy** is an in-house author at Packt Publishing. An engineering graduate from the Manipal Institute of Technology, his core interests are technical writing, web designing, and software testing. He was born and raised in Mumbai, India, and resides there. He is a music buff and loves listening to Oasis, R.E.M, The Doors, Dire Straits, and Eminem. Rock ’n’ Roll and Rap rule his playlists. He is a simple person by nature and believes that we are all here to have a good time, not a long time. After all, the most important thing is to be happy.

Although we have defined the <style> element as aravind.css, we have not used styling to avoid information overload. Also, the <script> element is not included to reduce complexity. We will see some styling in some of the code examples in this book, but we need to stick to the purpose of this book, which is to help you understand HTML better.

## Headings

Heading tags are used to differentiate the heading of a web page from the rest of the content. There is a hierarchy, which means the content in <h1> is the most important, and that in <h6> is the least important one.

Let's now look at a code example to understand it better:

```
<!DOCTYPE html>
<html>
<head>
<title> HTML Headings </title>
</head>
<body>
<h1>Welcome to Packt</h1>
<h2>Welcome to Packt</h2>
<h3>Welcome to Packt</h3>
<h4>Welcome to Packt</h4>
<h5>Welcome to Packt</h5>
<h6> Welcome to Packt </h6>
</body>
</html>
```

The output of the code on execution will be as follows:

![](img/b5ca7252918ddb25344f928f46a6669d_14_0.png)

The output of the code will be as follows:

![](img/b5ca7252918ddb25344f928f46a6669d_16_0.png)

From the output, you can see that `<strong>` is used to denote something important, whereas `<i>` is used for italic fonts. The `<code>` tag defines the computer code text, while `<em>` is used to describe something significant. The `<b>` tag is used for bold text. The `<sup>` and `<sub>` tags are used to define superscripts and subscripts. Superscripts and subscripts are basically those numbers or words that are smaller than the normal line type and appear slightly above and below it. Including comments is a good practice that ensures easy code readability. In HTML, comments are addressed between angular brackets with an exclamation mark, as shown in the following code:

```
<!-- Comments help aspiring newbies -->
```

Comments are not displayed in the browser.

## Attributes

HTML elements can have attributes that provide additional information about an element. They come in `<name: value>` pairs and are always a part of the opening tag. The value of the attribute must be enclosed in quotes for clarity (though it is not always necessary).

Let's look at the following code to understand it better:

```
```html
<!DOCTYPE html>
<html>
<head>
<title> The Packt Initiative </title>
<style>
#packtpub {
```
```

## Links

The anchor tag (`<a>`) in HTML is used to define hyperlinks. A **hyperlink** can be an element that will take you to a specific location, be it a website or any other place in the same document or any other document. We use the `href` attribute with the `<a>` tag to specify the link destination.

Let's look at the following code to understand it better:

```
```html
<!DOCTYPE html>
<html>
<head>
<title> Links </title>
</head>
<body>
<h1 id="packt"> Links in HTML </h1>
<p> A normal link </p>
<a href="http://packtlib.packtpub.com">Packt Library</a>
<br><hr>
<p> Image acts like a hyperlink </p>
<a href="http://www.packtpub.com/">
<img src="Packt-image2.png" alt="Packt" />
</a>
<br><hr>
<p> An email link </p>
<a href="mailto:xyz@packt.com">Send Email</a>
<hr>
<a href="#packt"> Go to the Heading tag </a>
</body>
</html>
```
```

The output of the code on execution will be as follows:

![](img/b5ca7252918ddb25344f928f46a6669d_19_0.png)

The output of the code on execution will be as follows:

![](img/b5ca7252918ddb25344f928f46a6669d_23_0.png)

If you observe the code and the output, the items listed under <ol> related to the Client-Side heading have numbered bullets, whereas the items listed under <ul> related to the Server-Side heading are in normal bullets. Each item is listed between the <li> and </li> tags.

## Forms

Forms are used to collect user input such as name, age, gender, and e-mail address by means of text, radio buttons, and checkboxes, to name a few. The input is then passed for processing on the server side. The most commonly used form attributes are name, action, target, and enctype (read as encryption).

Form input types come in various types such as text fields, password, radio buttons, checkboxes, and submit buttons. Between the <form> tags, you can specify the action attribute that tells the form where its contents will be sent to (mainly, a server-side script for processing). We use the method attribute that informs the form how the data is going to be sent; usually, it is GET by default, but you can also use POST and other methods.

An example of the GET method is the search engine results, where you can see the information you entered in your search box displayed in the web address field. The POST method is used for sensitive data where the form data is not visible to the user, thereby ensuring a high level of security.

Let's look at the following code to understand it better:

```
<!DOCTYPE html>
<html>
<body>
<h1> Motorbike Customer requirement </h1>
```

On the same web page, we defined another form. Let's look at the following line of code from the code sample:

```
<form action="MAILTO:abcxyz@demo.com" method="post" enctype="text/plain">
```

We defined the action where the feedback will be e-mailed to the concerned person whose e-mail address is abcxyz@demo.com. We used the POST method and plain text as the encryption type. We proceeded to define the input type as text and assigned a value that will be displayed in the text boxes to help the user know what information needs to be inserted into the boxes.

Later on, we defined a text area for the feedback field as we have a multiline input. We assigned 5 rows and 40 columns for the text area in this example. Thereafter, we created the **Send** and **Reset** buttons. The **Reset** button will clear the information you entered in the fields. Suppose we enter Edward as the name, Edward@packt.com as the e-mail address, and Miscellaneous in the comments field.

> Remember that you need to have an e-mail client such as Microsoft Outlook or Mozilla Thunderbird for this feature. In this example, we used Mozilla Thunderbird as the e-mail client.

When you click on **Send**, you can see the following output:

![](img/b5ca7252918ddb25344f928f46a6669d_26_0.png)

## HTML5 – the future is here

HTML5 is still in the development stage, and the standardized version is expected to be released later in 2014. HTML5 will soon be accepted as the benchmark. It differs from the previous versions of HTML in various ways. HTML5 works with modern browsers and also offers backward compatibility. It has a lot of new features that will eventually change the approach in designing websites. The document type declaration in HTML5 is very simple. All we need to do is type `<!DOCTYPE html>` so that the browser can recognize that we will be working with HTML5.
