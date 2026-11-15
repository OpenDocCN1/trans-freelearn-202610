![(missing alt)](cover/cover.jpg)

# Table of Contents

[Instant Backbone.js Application Development](#pr01.html) [Credits](#pr02.html) [About the Author](#pr03.html) [About the Reviewers](#pr04.html) [www.PacktPub.com](#pr05.html) [Support files, eBooks, discount offers and more](#pr05.html#ch00lvl1sec01) [packtlib.packtpub.com](#pr06.html) [Why Subscribe?](#pr06.html#ch00lvl2sec01) [Free Access for Packt account holders](#pr06s02.html) [1\. Instant Backbone.js Application Development](#ch01.html) [So, what is Backbone.js?](#ch01.html#ch01lvl1sec03) [Installation](#ch01s02.html) [Step 1 – what do I need?](#ch01s02.html#ch01lvl2sec03) [Step 2 – creating a folder](#ch01s02.html#ch01lvl2sec04) [Step 3 – downloading the necessary files](#ch01s02.html#ch01lvl2sec05) [Step 4 – creating new files](#ch01s02.html#ch01lvl2sec06) [And that's it!](#ch01s02.html#ch01lvl2sec07) [Quick start – building a Hello World app](#ch01s03.html) [Step 1 – adding code to the document](#ch01s03.html#ch01lvl2sec08) [Step 2 – adding code to the main script](#ch01s03.html#ch01lvl2sec09) [Step 3 – opening the project in your browser](#ch01s03.html#ch01lvl2sec10) [Step 4 – encountering a problem](#ch01s03.html#ch01lvl2sec11) [The missing script file](#ch01s03.html#ch01lvl3sec01) [The script typo](#ch01s03.html#ch01lvl3sec02) [Top 5 features you need to know about](#ch01s04.html) [Models and Collections](#ch01s04.html#ch01lvl2sec12) [Step 1 – creating some files and folders](#ch01s04.html#ch01lvl3sec03) [Step 2 – updating the main script](#ch01s04.html#ch01lvl3sec04) [Step 3 – updating the document](#ch01s04.html#ch01lvl3sec05) [Step 4 – updating the Contact model](#ch01s04.html#ch01lvl3sec06) [Step 5 – updating the Contact collection](#ch01s04.html#ch01lvl3sec07) [Step 6 – refreshing the browser](#ch01s04.html#ch01lvl3sec08) [Views and Templates](#ch01s04.html#ch01lvl2sec13) [Step 1 – creating some files and folders](#ch01s04.html#ch01lvl3sec09) [Step 2 – updating the Contact view](#ch01s04.html#ch01lvl3sec10) [Step 3 – updating the directory view](#ch01s04.html#ch01lvl3sec11) [Step 4 – updating the document](#ch01s04.html#ch01lvl3sec12) [Step 5 – updating the main script](#ch01s04.html#ch01lvl3sec13) [Step 6 – refreshing the browser](#ch01s04.html#ch01lvl3sec14) [Event listener examples](#ch01s04.html#ch01lvl3sec15) [View events](#ch01s04.html#ch01lvl2sec14) [Step 1 – getting ready](#ch01s04.html#ch01lvl3sec16) [Step 2 – updating the document](#ch01s04.html#ch01lvl3sec17) [Step 3 – updating the Contact view](#ch01s04.html#ch01lvl3sec18) [Step 4 – updating the directory view](#ch01s04.html#ch01lvl3sec19) [URL-based routing](#ch01s04.html#ch01lvl2sec15) [Step 1 – creating a router script](#ch01s04.html#ch01lvl3sec20) [Step 2 – updating the document](#ch01s04.html#ch01lvl3sec21) [Step 3 – updating the main script](#ch01s04.html#ch01lvl3sec22) [Step 4 – updating the Contacts view](#ch01s04.html#ch01lvl3sec23) [Step 5 – updating the directory view](#ch01s04.html#ch01lvl3sec24) [Persisting data to a server](#ch01s04.html#ch01lvl2sec16) [Step 1 – getting ready](#ch01s04.html#ch01lvl3sec25) [Step 2 – creating a server script](#ch01s04.html#ch01lvl3sec26) [Step 3 – updating the Contacts collection](#ch01s04.html#ch01lvl3sec27) [Step 4 – updating the main script](#ch01s04.html#ch01lvl3sec28) [Step 5 – updating the router](#ch01s04.html#ch01lvl3sec29) [Step 6 – updating the Contact view](#ch01s04.html#ch01lvl3sec30) [Step 7 – updating the directory view](#ch01s04.html#ch01lvl3sec31) [Step 8 – running the server](#ch01s04.html#ch01lvl3sec32) [People and places you should get to know](#ch01s05.html) [Official sites](#ch01s05.html#ch01lvl2sec17) [Community](#ch01s05.html#ch01lvl2sec18) [Twitter](#ch01s05.html#ch01lvl2sec19) 

* * *

Copyright © 2013 Packt Publishing

All rights reserved. No part of this book may be reproduced, stored in a retrieval system, or transmitted in any form or by any means, without the prior written permission of the publisher, except in the case of brief quotations embedded in critical articles or reviews.

Every effort has been made in the preparation of this book to ensure the accuracy of the information presented. However, the information contained in this book is sold without warranty, either express or implied. Neither the author, nor Packt Publishing, and its dealers and distributors will be held liable for any damages caused or alleged to be caused directly or indirectly by this book.

Packt Publishing has endeavored to provide trademark information about all of the companies and products mentioned in this book by the appropriate use of capitals. However, Packt Publishing cannot guarantee the accuracy of this information.

First published: July 2013

Production Reference: 1190713

Published by Packt Publishing Ltd.

Livery Place

35 Livery Street

Birmingham B3 2PB, UK.

ISBN 978-1-78216-566-8

[www.packtpub.com](http://www.packtpub.com)

**Author**

Thomas Hunter II

**Reviewers**

Kelly King

Jonathan Kuperman

**Acquisition Editor**

Edward Gordon

**Commissioning Editor**

Shaon Basu

**Technical Editor**

Sonali S. Vernekar

**Project Coordinator**

Amey Sawant

**Proofreader**

Stephen Copestake

**Production Coordinators** 

Conidon Miranda

Melwyn D’sa

**Cover Work**

Conidon Miranda

**Thomas Hunter II** is a Jack of all trades with over eight years of open source web development experience. This includes backend PHP / Node.js programming, MySQL / MongoDB database engineering, and frontend programming with JavaScript. He’s even done plenty of graphic design, game development, and music composition.

**Kelly King** is a frontend developer living in Ann Arbor, Michigan. Besides working as a JavaScript programmer at Systems in Motion, Kelly co-organizes a PHP user group, builds game and app mobile in her free time, and is earning her Master’s degree in Computer Science from Eastern Michigan University.

**Jonathan Kuperman** is a web developer currently living in Ann Arbor, Michigan. He works as a software engineer for Barracuda Networks and blogs at [www.insitedesignlab.com](http://www.insitedesignlab.com). He organizes and teaches at Business and Technology meetups in the area and spends his free time contributing to open source projects.

You might want to visit [www.PacktPub.com](http://www.PacktPub.com) for support files and downloads related to your book.

Did you know that Packt offers eBook versions of every book published, with PDF and ePub files available? You can upgrade to the eBook version at [www.PacktPub.com](http://www.PacktPub.com) and as a print book customer, you are entitled to a discount on the eBook copy. Get in touch with us at `<service@packtpub.com>` for more details.

At [www.PacktPub.com](http://www.PacktPub.com), you can also read a collection of free technical articles, sign up for a range of free newsletters and receive exclusive discounts and offers on Packt books and eBooks.

Do you need instant solutions to your IT questions? PacktLib is Packt's online digital book library. Here, you can access, read and search across Packt's entire library of books.

*   Fully searchable across every book published by Packt
*   Copy-and-paste, print and bookmark content
*   On demand and accessible via web browser

If you have an account with Packt at [www.packtpub.com](http://www.packtpub.com), you can use this to access PacktLib today and view nine entirely free books. Simply use your login credentials for immediate access.

![Free Access for Packt account holders](graphics/PacktLibLogo.jpg)

Welcome to the *Instant Backbone.js Application Development*. This book will guide you through the steps of downloading and installing Backbone.js and its dependent libraries, getting a simple Hello World app running to confirm your installation, and then finally going through each of the different classifications of code so that you can build a simple contact manager. You'll even build a quick server app to persist your data.

*So what is Backbone.js* explains exactly what Backbone.js is for, what sort of use cases you would need for it, who uses it, and the requirements.

*Installation* will guide you through acquiring the dependencies of Backbone.js, which need to be installed before you can use it in your project. This section will have you download and install them and set up a simple directory structure.

*Quick start – building a Hello World app* provides a simple Hello World message, to verify whether the installation has worked properly and our libraries are being loaded.

*Top 5 features you need to know about* covers the different classifications of code Backbone has to offer, including Models and collections, Views and their corresponding templates, Events, URL routing, and Persisting data to a server.

*People and places you should get to know* covers a good community of free resources out there that you should be aware of to take your Backbone development skills to the next level.

Backbone.js (or simply Backbone) is a frontend JavaScript framework for building data-heavy **Single Page Applications** (**SPAs**). You've probably heard of different framework patterns, such as **Model-View-Controller** (**MVC**), **Model-View-Presenter** (**MVP**), or even **Model-View-ViewModel** (**MVVM**), but Backbone follows its own special paradigm. Backbone doesn't require files to be structured in your filesystem in any sort of way, but it does have suggestions on how your project should structure code.

Backbone is most useful when your application uses a database in the backend to persist user data, and your frontend application will need to mirror a lot of this data. Assuming your data is accessible to your frontend app using a standardized format (RESTful JSON-based API over HTTP GET), and data can be persisted using the proper HTTP verbs (PUT, POST, and DELETE), working with Backbone to read and write data will be a breeze.

While running Backbone-based SPAs, your users will be interacting with data from the backend in their browser. When they interact with the application, they are doing so using Views. A View represents part of the **Document Object Model** (**DOM**), and is tied to a Model. When these interactions occur, they trigger Events, which update the data in the model. Backbone keeps track of these models and, when changes to data happen, it will send the data to your web server to be persisted. It also keeps track of changes to model data and can update your Views accordingly. This makes the process of updating the DOM and sending data to the server seem to be automatic. Instead of building several one-off jQuery `$.ajax` calls, a consistent method will be used instead.

First let's look at the reasons a web developer would want to use Backbone for their application. Many web developers begin building websites by throwing a bunch of jQuery calls in a single, monolithic JavaScript file. These calls do everything from querying the DOM to performing AJAX requests. When this happens, a project will usually get out of control and your application will be heavily tied to your DOM, which will mean lots of headaches when the designer comes up with a new trendy layout. One of the best reasons to use Backbone, in my opinion, is that it keeps your code organized. Many large web applications have been strangled by spaghetti code and difficulty in maintainability. A properly organized Backbone application will not have this pitfall.

If you do find yourself needing to change the DOM queries when a layout changes, they are kept together in the same file. If your database column names change and your frontend application stops working, changes could be made in a single file to get the frontend application compatible with the backend again.

Plenty of websites use Backbone for managing their frontend application! Here's a small list of popular web applications you've likely heard of to give you a taste:

*   Foursquare ([http://www.foursquare.com](http://www.foursquare.com))
*   Basecamp calendar ([http://basecamp.com/calendar](http://basecamp.com/calendar))
*   Airbnb ([http://airbnb.com](http://airbnb.com))
*   Stripe ([https://stripe.com](https://stripe.com))
*   Khan Academy ([http://www.khanacademy.org](http://www.khanacademy.org))
*   Pandora ([http://www.pandora.com/newpandora](http://www.pandora.com/newpandora))
*   Copy ([http://www.copy.com](http://www.copy.com))
*   And many more ([http://backbonejs.org/#examples](http://backbonejs.org/#examples))

To use Backbone.js, you will need at least Backbone itself, a utility library, which binds to the global variable `_` (the underscore character), and a selector engine, which binds to the global variable `$`. The de facto utility library is Underscore.js, and the de facto selector engine is jQuery.

The purpose of a utility library is to make up for some of the shortcomings of JavaScript. These libraries provide several functions to make programming faster. One such example is the `foreach` function, something you might have missed coming from the world of PHP development. The purpose of the selector engine is to provide a common method for querying DOM elements. Different browsers implement these in different ways, using a couple of different functions, and the results of the query aren't always the same. A good selector engine will abstract those complexities away from you and give you a single function, which accepts CSS-based selectors for performing queries.

Since I like choices, I'll mention some alternative libraries, which you can use for this project. LoDash is a drop-in replacement for Underscore.js and is faster, while Zepto is a drop-in replacement for jQuery and is smaller but doesn't work with Internet Explorer or older browsers.

## Step 1 – what do I need?

You will want to use a modern web browser while following the examples in this book. I recommend installing Google Chrome. It is the browser I will be using in my examples, is free, and has an amazing Web Inspector (in Apple OS: *Cmd* + *Option* + *I* and in Windows / Linux: *Ctrl* + *Shift* + *I*). The following screenshot shows us the opening console:

![Step 1 – what do I need?](graphics/5668OS_01_01.jpg)

You will also need the following third party scripts:

*   Backbone
*   Underscore or LoDash
*   jQuery or Zepto

## Step 2 – creating a folder

Create a folder on your computer for the project we'll be building with this book.

## Step 3 – downloading the necessary files

The next step is to download the following files:

*   The Backbone framework Backbone.js version 1.0.0 or later 
    
    *   Download the file from [http://backbonejs.org/backbone-min.js](http://backbonejs.org/backbone-min.js)
    *   Save the file as `scripts/backbone-1.0.0.js`
    
*   The utility library Underscore.js version 1.4.4 or later or LoDash version 1.3.1 or later 
    
    *   Download the file from [http://underscorejs.org/underscore-min.js](http://underscorejs.org/underscore-min.js)
    *   Save the file as `scripts/underscore-1.4.4.js`
    *   Or download the file from [https://raw.github.com/bestiejs/lodash/master/dist/lodash.min.js](https://raw.github.com/bestiejs/lodash/master/dist/lodash.min.js)
    *   Save the file as `scripts/lodash-1.3.1.js`
    
*   The selector engine jQuery version 1.9.1 or later or Zepto version 1.0 or later 
    
    *   Download the file from [http://code.jquery.com/jquery-1.9.1.min.js](http://code.jquery.com/jquery-1.9.1.min.js)
    *   Save the file as `scripts/jquery-1.9.1.js`
    *   Or download the file from [http://zeptojs.com/zepto.min.js](http://zeptojs.com/zepto.min.js)
    *   Save the file as `scripts/zepto-1.0.js`
    

## Step 4 – creating new files

Once you've got these files downloaded to your `scripts` folder within the `project` folder, you'll want to create another file inside of that directory called `main.js`, which will be where we put our own application-specific scripts.

While you're at it, create a file in the root of your `project` directory called `index.html`; this is where we'll add some HTML.

If all goes according to plan, you should see a folder layout similar to the one shown in the following screenshot:

![Step 4 – creating new files](graphics/5668OS_02_01.jpg)

## And that's it!

Congratulations, you've now got the necessary files in place for a Backbone-based project.

Now that you've got the necessary script files and a project directory laid out, let's begin writing some code to confirm that the application works. This code will require all scripts we've saved to be accessible; otherwise we'll see some error messages.

We'll also go over the Web Inspector and use it to interact with our applications. Learning this tool is essential for any web application developer to proficiently debug (and even write new code for) their application.

## Step 1 – adding code to the document

Add the following code to your `index.html` file. Notice that I'm assuming the use of LoDash and Zepto, so you'll want to update the code accordingly:

```
<!DOCTYPE HTML>
<html>
  <head>
    <title>Backbone Application Development Starter</title>
    <!-- Your Utility Library -->
    <script src="scripts/lodash-1.3.1.js"></script>
    <!-- Your Selector Engine -->
    <script src="scripts/zepto-1.0.js"></script>
    <script src="scripts/backbone-1.0.0.js"></script>
  </head>
  <body>
    <div id="display">
      <div class="listing">Houston, we have a problem.</div>
    </div>
  </body>
  <script src="scripts/main.js"></script>
</html>
```

### Tip

**Downloading the example code**

You can download the example code files for all Packt books you have purchased from your account at [http://www.packtpub.com](http://www.packtpub.com). If you purchased this book elsewhere, you can visit [http://www.packtpub.com/support](http://www.packtpub.com/support) and register to have the files e-mailed directly to you.

This file will load all of our scripts and has a simple `<div>`, which displays some content.

Notice that I have placed the loading of our `main.js` file after the closing `<body>` tag. This is just a personal preference of mine; it will ensure that the script is executed after the elements of the DOM have been populated. If you were to place it adjacent to the other scripts, you would need to encapsulate the contents of the script with a function call so that it gets run after the DOM has loaded; otherwise, when the script runs and tries to find the `div#display` element, it will fail.

## Step 2 – adding code to the main script

Now, add the following code to your `scripts/main.js` file:

```
var object = {};

_.extend(object, Backbone.Events);

object.on("show-message", function(msg) {
  $('#display .listing').text(msg);
});

object.trigger("show-message", "Hello World");
```

Allow me to break down the contents of `main.js` so that you know what each line does.

```
var object = {};
```

The preceding line should be pretty obvious. We're just creating a new object, aptly named `object`, which contains no attributes.

```
_.extend(object, Backbone.Events);
```

The preceding line is where the magic starts to happen. The utility library provides us with many functions, one of which is `extend`. This function takes two or more objects as its arguments, copies the attributes from them, and appends them to the first object. You can think of this as sort of extending a class in a classical language (such as PHP).

Using the `extend` utility function is the preferred way of adding Backbone functionality to your classes. In this case, our `object` object is now a Backbone `Event` object, which means it can be used for triggering events and running callback functions when an event is triggered. By extending your objects in this manner, you have a common method for performing event-based actions in your code, without having to write one-off implementations.

```
object.on("show-message", function(msg) {
  $('#display .listing').text(msg);
});
```

The preceding code adds an event listener to our object. The first argument to the `on` function is the name of the event, in this case `show-message`. This is a simple string that describes the event you are listening for and will be used later for triggering the events. There isn't a requirement as far as naming conventions go, but try to be descriptive and consistent.

The second argument is a callback function, which takes an argument that can be set at the time the event is triggered. The callback function here is pretty simple; it just queries the DOM using our selector engine and updates the text of the element to be the text passed into the event.

```
object.trigger("show-message", "Hello World");
```

Finally, we trigger our event. Simply use the `trigger` function on the object, tell it that we are triggering the `show-message` event, and pass in the argument for the trigger callback as the second argument.

## Step 3 – opening the project in your browser

This will show us the text **Hello World** when we open our `index.html` file in our browser. Don't believe me? Double click on the file now, and you should see the following screen:

![Step 3 – opening the project in your browser](graphics/5668OS_03_01.jpg)

If Chrome isn't your default browser, you should be able to right-click on the file from within your file browser and there should be a way to choose which application to open the file with. Chrome should be in that list if it was installed properly.

## Step 4 – encountering a problem

Do you see something other than our happy **Hello World** message? The code is set up in a way that it should display the message **Houston**, **we have a problem** if something were to go wrong (this text is what is displayed in the DOM by default, before having JavaScript run and replace it).

### The missing script file

The first place to look for the problem is the **Network** tab of the Web Inspector. This tab shows us each of the files that were downloaded by the browser to satisfy the rendering of the page. Sometimes, the **Console** tab will also tell us when a file wasn't properly downloaded, but not always. The following screenshot explains this:

![The missing script file](graphics/5668OS_03_02.jpg)

If you look at the **Network** tab, you can see an item clearly marked in red (**backbone-1.0.0.js**). In my project folder, I had forgotten to download the Backbone library file, thus the browser wasn't able to load the file. Note that we are loading the file from the local filesystem, so the status column says either **Success** or **Failure**. If these files were being loaded from a real web server, you would see the actual HTTP status codes, such as **200 OK** for a successful file download or **404 Not Found** for a missing file.

### The script typo

Perhaps you have made a typo in the script while copying it from the book. If you have any sort of issues resulting from incorrect JavaScript, they will be visible in the **Console** tab, as shown in the following screenshot:

![The script typo](graphics/5668OS_03_03.jpg)

In my example, I had forgotten the line about extending my object with the Backbone events class. Having left out the `extend` line of code caused the object to be missing some functionality, specifically the method `on()`. Notice how the console displays which filename and line number the error is on.

Feel free to remove and add code to the files and refresh the page to get a feel for what the errors look like in the console. This is a great way to get a feel for debugging Backbone-based (and, really, any JavaScript) applications.

The following sections will guide you through building a simple contact manager app. You will write different types of code, keeping the code organized into different files and directories depending on the purpose of the code.

## Models and Collections

Collections represent database tables, and models represent individual rows within that table. With our application, we're going to be storing a bunch of people's contact information in our address book. We will have a contacts collection for all of our contacts and a different contact model for each person in our collection.

### Step 1 – creating some files and folders

Create two new directories in our `scripts` directory, one called `models` and the other called `collections`.

Create a new file named `scripts/models/contact.js`.

Create a `scripts/collections/contact.js` file.

### Step 2 – updating the main script

Replace all of the code in `scripts/main.js` with the following:

```
var App = {
  Models: {},
  Collections: {},
  Contacts: null
};

$(function() { // Run this code when the DOM is ready
  window.tom = new App.Models.Contact({
    firstName: 'Thomas',
    lastName: 'Hunter',
    phoneNumber: '9895135499',
    email: 'me@thomashunter.name'
  });

  App.Contacts = new App.Collections.Contact();

  App.Contacts.add(window.tom);

  App.Contacts.add({
    firstName: 'Rupert',
    lastName: 'Styx',
    phoneNumber: '9895551212',
    email: 'rupertstyx@example.com'
  });

  App.Contacts.add({});

  var contactListing = '';
  App.Contacts.each(function(contact) {
    contactListing += "<div>" +
    contact.get('firstName') + " " +
    contact.get('lastName') + " ";

    if (contact.isValid()) {
      contactListing += "(valid)";
    } else {
      contactListing += "(invalid)";
    }
    
    contactListing += "</div>";
  });
  $('#display').html(contactListing);

});
```

### Step 3 – updating the document

Add the following two script tags to the `index.html` document, after the existing `<script>` tag near the closing `<body>` tag:

```
<script src="scripts/models/contact.js"></script>
<script src="scripts/collections/contact.js"></script>
```

### Step 4 – updating the Contact model

Add the following code to the `scripts/models/contact.js` file:

```
App.Models.Contact = Backbone.Model.extend({
  defaults: {
    firstName: '',
    lastName: '',
    phoneNumber: '<UNLISTED>',
    email: '<UNLISTED>'
  },

  validate: function(attrs, options) {
    if (!attrs.firstName) {
      return "A valid contact must have a first name";
    }
  },

  initialize: function(attributes) {
    var firstName = attributes.firstName || '<EMPTY>';
    var lastName = attributes.lastName || '<EMPTY>';
    console.log("Initializing a new contact model for '" +
      firstName + " " + lastName + "'."
    );
  }
});
```

### Step 5 – updating the Contact collection

Add the following code to `scripts/collections/contact.js`:

```
App.Collections.Contact = Backbone.Collection.extend({
  model: App.Models.Contact
});
```

### Step 6 – refreshing the browser

Once you're done adding the preceding code, refresh your browser window. You should see the following screen in your browser:

![Step 6 – refreshing the browser](graphics/5668OS_04_01.jpg)

After running this code, we've successfully created our collection and model objects, then instantiated a collection as well as populated it with three different models. We also iterated over each item within the collection, much like we would with a database result, and displayed them one at a time on the screen.

Let us have a look at `scripts/main.js`. This file contains our application bootstrapping code:

```
var App = {
  Models: {},
  Collections: {},
  Contacts: null
};
```

This code creates a global object called `App`, which contains three attributes. The first two, `Models` and `Collections`, are empty objects that will contain our `Model` and `Collection` objects that have extended the Backbone Model and Collection object, respectively. Think of these as templates for later creating instances that we can work with. In JavaScript, there are no classes, only objects, but these will contain objects we will pretend are classes.

The third attribute, `Contacts`, is where we will actually store our main Contact collection instance. Since we will only ever need one of them, and having it globally accessible is convenient, we assign it here. Notice that we don't keep track of individual Contact model instances in this manner.

Creating a global object called `App` isn't actually required by Backbone. This is a convention that I use so that I can later interact with this global object from different scripts. If you were creating your entire application within a single file, you could simply create temporary local variables to point to these objects.

```
$(function() { // Run this code when the DOM is ready
```

The next bit of code in `main.js` is executed after the DOM loads. The reason for this is a sort of catch 22. The different `Model` and `Collection` files we instantiate after calling `main.js` need access to the global `App` variable. The majority of our `main.js` code requires the `Collections` and `Models` objects to have been attached to the global `App` object. Running the code after the DOM loads ensures that the `Model` and `Collection` scripts load first, and we never run into uninitialized variable problems.

```
  window.tom = new App.Models.Contact({
    firstName: 'Thomas',
    lastName: 'Hunter',
    phoneNumber: '9895135499',
    email: 'me@thomashunter.name'
  });
```

Here we create an instance of our Contact Model code. When instantiating the object, we pass in a simple object that will be converted into the more complex Backbone model. Notice how I am creating a global variable named `tom`. The reason I did this is that we can inspect it easily in the Web Inspector.

To see what the object looks like, open up your **Console**, simply type `tom`, and press *Enter*. You can then expand some of the attributes to see what the object looks like, as shown in the following screenshot:

![Step 6 – refreshing the browser](graphics/5668OS_04_02.jpg)

As you can see, Backbone adds a lot of data for us. This data is used by Backbone to help keep track of things we change, event bindings, which data to send to the server when we're ready to persist, and so on.

The most interesting part in here is the `attributes` attribute, which contains all of that simple data we added before. You could access this information by running `tom.attributes.firstName`, but you should really access it instead by running `tom.get('firstName')` as a convention.

```
App.Contacts = new App.Collections.Contact();
```

The preceding line creates a globally accessible instance of the Contact Collection, assigned to `App.Contacts`. Later in our application when we want to work with our different contacts, we just have to type `App.Contacts`.

```
  App.Contacts.add(window.tom);

  App.Contacts.add({
    firstName: 'Rupert',
    lastName: 'Styx',
    phoneNumber: '9895551212',
    email: 'rupertstyx@example.com'
  });

  App.Contacts.add({});
```

The preceding lines of code add three different contacts to our Contact Collection. The first one takes the instance of the Model we created beforehand and adds it to the collection.

The second one implicitly creates a new model for us from the simple object we supply. Obviously it can save us a few lines of code by doing it this way. However, sometimes you want to perform some model-specific actions on your data before inserting it into the Collection. Perhaps you want to perform some validation first. In those cases, create the model ahead of time; otherwise, just do it this way.

The third one also implicitly creates a new model, but notice how none of the attributes has been set. We add this empty Model so we can see how validation and default values work later.

```
  var contactListing = '';
  App.Contacts.each(function(contact) {
    contactListing += "<div>" +
    contact.get('firstName') + " " +
    contact.get('lastName') + " ";

    if (contact.isValid()) {
      contactListing += "(valid)";
    } else {
      contactListing += "(invalid)";
    }
    
    contactListing += "</div>";
  });
  $('#display .listing').html(contactListing);
```

This code simply iterates over every Contact Model we have in our Collection and displays them on screen. For performance I append values to a string instead of continuously appending to the DOM.

The `Collection.each()` function allows us to iterate over each item in the collection, and we provide it with a callback function to be executed for each iteration. The argument to this function is the value of the item being iterated upon. In our case, each time the callback executes it contains a Contact model.

When we want to get the value of an attribute of the model, we use `Model.get()`, where we supply the name of the attribute. This is preferable to accessing Model attributes directly, as we can run events when accessing data. Think of it as using a public class variable instead of a private or protected class variable in other languages.

The `Model.isValid()` method performs a check to see if our model is valid. This runs the `Model.validate()` function we define in our Model template code and is useful for performing validation on user-provided input. If the function returns a truthy value (for example, an error string), the validation fails.

Let us now have a look at `index.html`. This file loads our scripts.

```
<script src="scripts/models/contact.js"></script>
<script src="scripts/collections/contact.js"></script>
```

The code we added here should be self-explanatory. We need to access the JavaScript in our newly created files, and this allows us to do so. Alternatively we could have kept all code in one file, or made use of script-loading technologies such as RequireJS, but that is beyond the scope of this book.

Now let's look at `scripts/models/contact.js`.

Code in this file will be specific to handling the Contact Model.

```
App.Models.Contact = Backbone.Model.extend({
  defaults: {
    firstName: '',
    lastName: '',
    phoneNumber: '<UNLISTED>',
    email: '<UNLISTED>'
  },
```

This code extends the Backbone Model object with our own properties and assigns it to the globally accessible `App.Models.Contact` object. Here we specify some default values for our contact models. In this case, every one of our defaults is a string. Typically, you'll want to keep track of the types of variables you have assigned to your attributes, since you're probably persisting data on the backend in some sort of relational database. Backbone doesn't care if you change types, though.

Whenever you create a new Contact model, any unspecified values will become the default value specified in the preceding code.

```
  validate: function(attrs, options) {
    if (!attrs.firstName) {
      return "A valid contact must have a first name";
    }
  },
```

This is our validation method, which can be used to see if the data provided to a model is valid or not. The first argument to this method is the attributes, which contains the attributes being provided to the model. You can run any type of JavaScript logic you want in here, for example, checking variable types or string lengths. In this code we simply check if the `firstName` is a truthy value (for example, not an empty string).

This method, when returning a truthy value (for example, a string describing the error), will mean that the validation has failed. This way, you can provide user-readable errors that the validation has failed. It will be up to you to display the failure message to the user. You can get this value by accessing `Model.validationError`.

```
  initialize: function(attributes) {
    var firstName = attributes.firstName || '<EMPTY>';
    var lastName = attributes.lastName || '<EMPTY>';
    console.log("Initializing a new contact model for '" +
      firstName + " " + lastName + "'."
    );
  }
```

This method is executed each time a new Contact model is instantiated. Notice that we're checking to see if the specific attributes have been provided or not. The reason for doing so is because `attributes` contains only the values passed into the model during creation; it will not contain any of the defaults. This method does however display a console message to help us debug our application development.

Let us now take a look at `scripts/collections/contact.js`. Code in this file will be specific to handling the Contact Collection.

```
App.Collections.Contact = Backbone.Collection.extend({
```

Again, we begin the file by extending the specific Backbone object.

```
  model: App.Models.Contact
```

This line tells our Backbone collection that it will contain a listing of Contact Models. For this code to work, the Contact Model code needs to have been loaded by the browser. This is why we loaded the model file before loading the collection file.

## Views and Templates

A view represents a DOM element, as well as the child elements within that element. A template is a bunch of HTML with special syntax embedded within it and allows you to easily create your views.

Templates need to be accessible to your scripts in some form or another. Technically, all they are is a string of text. The most common method for providing templates to ones script is to embed them in the main SPAs DOM through the use of `<script>` tags. This will likely surprise you, as `<script>` tags are typically used for loading JavaScript. However, there's a reason for that `type` attribute. Several years ago it wasn't uncommon to embed Visual Basic scripts for Internet Explorer to execute, and one day we may see browsers executing Dart directly.

By simply specifying a type the browser hasn't heard of, the browser shouldn't attempt to execute the code within the `<script>` tag. By providing an `id` attribute on the `<script>` tag, we can access its content using our code. The reason we don't do something such as storing the template code in a hidden `textarea` is because double quotes in the template would need to be escaped and that's a little messy. We also don't store templates as hidden elements in the DOM because it would be unnecessary to complicate the DOM with unused template elements.

While working with Views, we'll make use of the template rendering functionality built-in to Underscore.js. This template renderer is inspired by the Ruby ERB syntax. If your HTML contains `<%= variable %>`, it will print out the value of variable. If you would like to escape that code to prevent HTML injection, use `<%- variable %>`. You can even go as far as to run arbitrary JavaScript code by using `<% expression() %>`.

### Step 1 – creating some files and folders

Create a new directory in `scripts` called `views`, as well as two new files `scripts/views/contact.js` and `scripts/views/directory.js`.

### Step 2 – updating the Contact view

Add the following code to `scripts/views/contact.js`:

```
App.Views.Contact = Backbone.View.extend({
  template: _.template($('#template-contact').html()),
  $container: null,

  initialize: function(options) {
    _.bindAll(this, 'render', 'insert');

   this.$container = options.$container;

    this.listenTo(this.model, 'change', this.render);
    this.insert();
  },

  render: function() {
    this.$el.html(this.template(this.model.attributes));

    return this;
  },

  insert: function() {
    this.$container.append(this.$el);
  }
});
```

### Step 3 – updating the directory view

Add the following code to `scripts/views/directory.js`:

```
App.Views.Directory = Backbone.View.extend({
  initialize: function() {
    _.bindAll(this, 'render');
  },

  render: function() {
    var $container = this.$('.listing').empty();

    App.Contacts.each(function(contact) {
      new App.Views.Contact({
        model: contact,
        $container: $container
      }).render();
    });

    return this;
  }
});
```

### Step 4 – updating the document

Add the following code to `index.html`, after the existing `<div>` tag:

```
<script id="template-contact" type="template">
  <div class="contact">
    <h2><%-firstName%><% if (lastName) { %> <%-lastName%><% } %></h2>
    <a href="mailto:<%-email%>"><%-email%></a><br />
    <a href="tel:<%-phoneNumber%>"><%-phoneNumber%></a><br />
    <button class="delete">Delete</button>
  </div>
</script>
```

Also, add the following code to `index.html`, after the existing `<script>` tags after the closing `<body>` tag:

```
<script src="scripts/views/contact.js"></script>
<script src="scripts/views/directory.js"></script>
```

### Step 5 – updating the main script

Update the `App` object in `main.js` so that it looks like the following:

```
var App = {
  Models: {},
  Collections: {},
  Views: {},
  Contacts: null,
  Directory: null
};
```

Also, in `main.js`, remove the code where we add an empty contact, as well as the code where we iterate over the contacts and display them in the DOM.

Replace that code with the following new code:

```
  App.Directory = new App.Views.Directory({
    el: $('#display')
  });

  App.Directory.render();

  App.Contacts.on('add remove', function() {
    App.Directory.render();
  });
```

### Step 6 – refreshing the browser

Once you've added all the new code, go back to your browser and refresh the page. You should see something like the following screenshot:

![Step 6 – refreshing the browser](graphics/5668OS_04_03.jpg)

As you can see, we're making use of two Views in this application now, even though we've only got one Collection and one Model. This will happen a lot in your Backbone applications; there isn't necessarily a one-to-one mapping between the different types of classes. In our code, the directory view represents our main application view. It will always represent a single element, which will always exist in the page. The Contact views, however, might not exist at all or could exist many times.

Once again we will have a look at the code in detail. Let us first have a look at `scripts/views/contact.js`. This code is related to our individual Contact view.

```
App.Views.Contact = Backbone.View.extend({
```

Again, we start by extending the equivalent built-in Backbone object.

```
  template: _.template($('#template-contact').html()),
  $container: null,
```

The template attribute is a Backbone convention for storing the template object. In this case, you can see that we're searching the DOM for an element identified as `#template-contact` (the `<script>` tag we added earlier), then grabbing the HTML contained within it. We then pass this HTML to the `_.template` method, which parses the string and performs some work on it, sort of compiling the template. This way, the CPU-expensive calculation is done once and we can use it later.

The `$container` attribute will point to the element that our Contact views will live inside of. This isn't a Backbone convention, just a convenience method.

```
  initialize: function(options) {
    _.bindAll(this, 'render', 'insert');

    this.$container = options.$container;

    this.listenTo(this.model, 'change', this.render);
    this.insert();
  },
```

The `initialize()` method is run every time this view is instantiated, and therefore resembles a constructor in many class-based languages. One is able to pass in extra parameters when instantiating the class. As you can see in the preceding code, we're referring to this argument as `options`, although that isn't necessarily a Backbone convention.

The `_.bindAll()` method is a convenience function for quickly specifying a bunch of other methods of this object we would like to overwrite. The preceding code will rebind this to refer to the entire View object, instead of just the local method it is being used in (either `render()` or `insert()`).

One of the options we pass into our View objects is the containing element that they should be added to, so we take this element and assign it to our class as `$container` and refer to it later.

The line where we call `listenTo()` is where the real magic starts to happen. When instantiating a View, we can pass in a model attribute, which is then assigned as the model for that view (Backbone handles this assignment for us; we don't have to do it manually in the `initialize` method). The View then adds an event handler to the model so that whenever the data of that model is updated, the View is notified and the `render()` method is executed. This is extremely useful and prevents us from having to write ad-hoc DOM-updating code everywhere we have the possibility of updating data.

After that, we run the `insert()` method, which will insert our element into our container. We don't do this in the `render()` method, since we don't want to keep inserting our element into the container unnecessarily!

```
  render: function() {
    this.$el.html(this.template(this.model.attributes));

    return this;
  },
```

This is the `render()` method, and is a convention of Backbone. It is executed every time we want our View to update its DOM element. In our example, we simply take our models attributes (remember inspecting it earlier and seeing it is where the data is stored in the model), pass it to the locally cached template, get the resulting HTML, and assign it to the `$el` attribute on our View object. `$el` is a Backbone convention for storing a reference to the current Views DOM element. Lastly we return a reference to the object to make chaining easier.

```
  insert: function() {
    this.$container.append(this.$el);
  }
```

This code simply appends our element to the containing element. If we ran this code every single time we ran `render()`, our elements would continually bounce out of order.

Next we shift our attention to `scripts/views/directory.js`. This code is related to our main application view.

```
App.Views.Directory = Backbone.View.extend({
  initialize: function() {
    _.bindAll(this, 'render');
  },
```

This should all be self-explanatory. We extend the Backbone view object and have an `initialize` function that just handles binding the `this` variable.

```
  render: function() {
    var $container = this.$('.listing').empty();
```

The first thing our `render()` method does in this view is clear out the existing elements. The first time this function is run it will remove our **Houston**, **we have a problem** text. Subsequent calls will remove the individual contact views markup.

```
    App.Contacts.each(function(contact) {
      new App.Views.Contact({
        model: contact,
        $container: $container
      }).render();
    });
```

This code takes our Contact collection and iterates over each individual contact. While doing so, we instantiate a new View for said contact. The model attribute we pass in is the model we are iterating on from the Collection. We also pass in a reference to this view's main element, which is to be the `$container` for our subviews.

Once our Views are instantiated, we immediately run their `render()` method, and they become visible in the DOM.

We will next have a look at `scripts/main.js`. These changes give us a nicer way to iterate over contacts and display them in the DOM.

```
var App = {
  Models: {},
  Collections: {},
  Views: {},
  Contacts: null,
  Directory: null
};
```

Our global `App` object gets a `Views` attribute for storing references to our views. The `Directory` attribute is a reference to the instance of the main application view. Since there's only one and it is rather important, having a global reference makes it easier to work with (and debug in the console).

```
  App.Directory = new App.Views.Directory({
    el: $('#display')
  });
```

This code performs the `Directory` view instantiation, passing in a reference to the existing DOM element we would like it to attach to.

```
  App.Directory.render();
```

After we instantiated the `Directory` View, we go ahead and execute the `render` function, which ends up running the `render` function of each Contact in our Contacts Collection.

```
  App.Contacts.on('add remove', function() {
    App.Directory.render();
  });
```

This code adds two event listeners to the Contacts collection, which are triggered when a Contact has been either added or removed. With this syntax, we can specify as many events as we would like, separated by spaces. As the events fire, the Directory re-renders, displaying the updated content.

What this means is, whenever we add or remove an item, the DOM updates for us automatically! This reduces ad-hoc jQuery calls and disorganized code by quite a bit.

Finally we look at `index.html`. These changes reflect the new template `<script>` tag, and a few `<script>` tags to load our new files.

```
    <script id="template-contact" type="template">
```

Storing HTML in a `<script>` tag makes it easier to access the content without having to deal with escaping issues. The `id` attribute gives us a handle to reference the code with. It's a good idea to prefix your template elements with a common namespace, in this case we refer to them all as `#template-`. Otherwise, we might later add an element called `#contact` and get things confused. The `type` attribute is set to `template`, which is a language our browser doesn't know how to interpret, and therefore ignores it.

```
      <div class="contact">
  <h2><%-firstName%><% if (lastName) { %> <%-lastName%><% } %></h2>
```

### Note

It's a good idea to always surround your templates in a single element, instead of having several top-level elements adjacent to each other. This way, when we go to delete the view from the DOM later, we only have one element to remove as the child elements will automatically be removed.

Here we are printing a variable for the contacts first name, and then checking to see if they have a last name. If they do, we print that one as well; otherwise we don't do anything.

If you've worked with other template languages before, you likely already know this: be sure to not put a lot of logic inside of your View files, otherwise they can get a bit out of control. The biggest benefit of breaking an application into different pieces such as models and views is that we keep an obvious distinction between which type of code goes where, and lots of logic in views will break this. The types that are okay to run are simple looping and conditional statements, perhaps some helper style functions that reduce writing the same markup everywhere, and things such as that.

```
      <a href="mailto:<%-email%>"><%-email%></a><br />
      <a href="tel:<%-phoneNumber%>"><%-phoneNumber%></a><br />
```

These two lines will display the e-mail address and phone number as clickable links to tell the device to send an e-mail or place a phone call, respectively. Nothing too special here, we're just making use of conventional browser anchor tag prefixes.

```
        <button class="delete">Delete</button>
      </div>
    </script>
```

Here is a **Delete** button for the contact; however, it is currently non-functioning. We then close the `View` element and the `<script>` tag containing it.

### Event listener examples

Now that you've seen the document for the page rendered based on the data we entered to begin with, it would be really nice to see how dynamic Backbone is, and how we can update data later and see how the resulting DOM changes.

In the contact View `initialize` function, we ran the `this.listenTo()` method, which looks for changes to the model before re-executing the `render()` method. To see this in action, open your console then type and execute the following command:

```
App.Contacts.get('c1').set('lastName', 'Wiggum');
```

You should immediately see the document change. The last name of the first contact has been changed to say **Wiggum**! This change is both reflected in the Model, and in the DOM itself. You didn't have to run any ad hoc jQuery commands or anything!

### Note

`c1` is a Backbone convention to refer to the first item of a collection. There are better ways to refer to these objects, such as a real ID value or keeping a reference to the Model upon creation.

In our main JavaScript file, we added the event listener to the Contacts collections, so that , whenever a new contact is added, it runs the Directory Views `render()` method. To prove this works, type the following command into your JavaScript console and execute:

```
  App.Contacts.add({
    firstName: 'Wednesday',
    lastName: 'Addams',
    phoneNumber: '9895551313',
    email: 'wednesday@example.com'
  });
```

### Tip

If you want to enter multiple lines of a command in this console, press *Shift* + *Enter* after each command, as pressing *Enter* will execute the incomplete command.

You should see the document immediately update and display the new contact!

Finally, if you'd like to see what happens when a contact is removed, you can run the following in your console:

```
App.Contacts.pop();
```

The template rendering engine, which comes with Underscore.js, is decent enough. If you like greater than, less than, and percent symbols, you will love it. However, if you prefer a different syntax, I would recommend doing some research on an alternative template engine called Mustache.

The Mustache engine uses curly brackets for defining variables, and conveniently has an implementation library in several popular programming languages, such as PHP and Ruby. This is convenient because as a developer you can learn a single template engine and use it everywhere.

Many other alternative engines exist such as Handlebars, which is pretty similar to Mustache, and HAML, which has a very terse syntax for easily creating complex HTML documents.

## View events

Events are a method for taking user interactions in a view, and running code when they happen. Events are specified using a syntax that looks something like `click .controls .add`. The first part of the string (up until the first whitespace) is the type of event we're looking for, in this case a `click` event. Everything after that is a selector.

With our preceding example, if we were doing traditional jQuery calls, we might have done a search for every element with an `add` class located inside an element with a `controls` class, and added a `click` event handler. There are two things wrong with this. First, as the DOM changes, we need to run this code again; otherwise, new elements matching the selector would be missed. Second, we could possibly have hundreds of event listeners listening on the page at once, causing it to slow down.

Remember that a view points to a single element in the DOM, even though that element may contain many more elements. Event bindings are applied to the main element, and use something called event delegation to check when the proper event has been called. So, with the preceding example event, the entire view listens for a `click` event on any of the children elements. When that event occurs, the element that triggered the event is matched against the selector. If it passes then our code is executed; otherwise, it is ignored.

### Step 1 – getting ready

Before jumping right into the coding bit, let's talk a little bit about what our application should be able to do, and think a little bit about responsibilities of code. We want to be able to add new contacts to our directory. When adding an item, that bit of responsibility lies outside of any specific contact. It's something we sort of do for the entire directory. For that reason, let's put the Add New Contact button inside of the Directory view. Also, we want to be able to delete existing contacts. Those definitely are a per-contact item, as each contact will need its own delete button. For this reason we'll add a delete button to the Contact views.

### Step 2 – updating the document

Update the `index.html` file with the following code. Place it inside of `div#display`, before `div.listing`:

```
      <div class="controls">
        <button class="add">Add New Contact</button>
        <form method="" action="" style="display: none;">
          <input type="text" placeholder="First Name" class="firstName" /><br />
          <input type="text" placeholder="Last Name" class="lastName" /><br />
          <input type="text" placeholder="Email" class="email" /><br />
          <input type="text" placeholder="Phone" class="phoneNumber" /><br />
          <input type="submit" value="Save Contact" />
        </form>
      </div>
```

### Step 3 – updating the Contact view

Update the `scripts/views/contact.js` file with the following code before the `initialize` method:

```
  events: {
    'click .delete': 'remove'
  },
```

Also in this file, add another method at the end of the main View object (you'll want to add a trailing comma after the `insert()` method preceding it):

```
  remove: function() {
    this.model.destroy();
  }
```

Be sure to update the `_.bindAll()` call in `initialize` so that it includes the `remove` method:

```
    _.bindAll(this, 'render', 'insert', 'remove');
```

### Step 4 – updating the directory view

Update the `scripts/views/directory.js` file by adding the following code near the top of the main View object (before the initialize method):

```
  events: {
    'click .controls .add': 'addForm',
    'submit .controls form': 'addSubmit'
  },
```

Also in this file, add the following methods at the end of the main View object, after the existing `render()` method (and don't forget that trailing comma after the `render` closing brace):

```
  addForm: function() {
    this.$('.controls form').show()
      .find('input.firstName').focus();
  },

  addSubmit: function(event) {
    event.preventDefault();
     var $form = this.$('.controls form');

    var newContact = new App.Models.Contact({
      firstName: $('input.firstName', $form).val(),
      lastName: $('input.lastName', $form).val(),
      phoneNumber: $('input.phoneNumber', $form).val(),
      email: $('input.email', $form).val()
    });

    if (newContact.isValid()) {
      App.Contacts.add(newContact);
      $form.hide();
      $('input[type=text]', $form).val('').blur();
    } else {
      alert(newContact.validationError);
    }
  }
```

Be sure to also add `addSubmit` and `addForm` to the `_.bindAll()` call in initialize:

```
    _.bindAll(this, 'render', 'addForm', 'addSubmit');
```

The changes we made this time allow us to interact with our Collection of contacts using standard form controls.

Let us have a look at `index.html`:

```
      <div class="controls">
        <button class="add">Add New Contact</button>
```

The new `div.controls` element will contain all of the controls and inputs for creating new contacts within our application. The button will be used for toggling the visibility of the form for adding contacts.

```
<form method="" action="" style="display: none;">
```

The `<form>` tag here won't actually post anything to the server, so we can leave the `method` and `action` attributes empty. We really just want to use it to keep track of when the user is attempting to submit their new data. Remember, there are two ways a user will typically submit a form. One is by pressing *Enter*, the other by clicking on a **Submit** button.

### Tip

As a general JavaScript and HTML pro-tip, do not add events, which listen for a button click or for keystrokes, and attempt to look for the *Enter* key being pressed.

For one thing, you are looking for two different user actions. For another thing, the user could be submitting the form via another method, such as screen reading software or a mobile device, which has a special submit form button attached to the keyboard.

We want the form to be hidden by default, so I've added some inline CSS to make it hidden (if this were a real application, you'd use an external CSS file to handle this, but I kept it inline for brevity).

```
          <input type="text" placeholder="First Name" class="firstName" /><br />
          <input type="text" placeholder="Last Name" class="lastName" /><br />
          <input type="text" placeholder="Email" class="email" /><br />
          <input type="text" placeholder="Phone" class="phoneNumber" /><br />
```

These are the inputs where the user will type their information. Notice how I added the `type` attributes and set them to `text`. Normally this is done automatically but, when left out, the selector used elsewhere in the code to clear the elements wouldn't target the elements properly.

```
          <input type="submit" value="Save Contact" />
        </form>
      </div>
```

Of course, we want a submit button, so that we can run our code when the form is submitted. Without this button, pressing *Enter* will not submit the form. Of course we also want to end the form and the View element tags too.

We next look at `scripts/views/contact.js`:

```
  events: {
    'click .delete': 'remove'
  },
```

The `events` attribute on View objects is a Backbone convention for specifying which events to listen for and which methods to execute when they are triggered. This attribute is an object with different key/value pairs for each event being listened for. The key represents the special event delegation syntax, and the value is a string with the name of the method to be executed.

In this case, when an element with a class of `delete` is clicked, the `remove` method is executed. Make sure that you're using a string to represent the name of the method, not an actual reference to the method such as `this.remove`.

When the event is triggered and the code is executed, the first argument to the method will be the event object, which you can perform further examination of (for example, if you want to find out exactly which element in the View triggered the event).

```
  remove: function() {
    this.model.destroy();
  }
```

This code will execute the `destroy()` method on the Model. This method is provided by Backbone for performing deletes on your data. It will handle not only deleting the model, but also removing it from the collection.

Backbone is actually smart enough to take it a step further. When this code is executed, it will even remove the relevant contact element from the DOM for us. Open the Element Inspector of Chrome, and navigate to the `.listing` element to keep an eye on the DOM while deleting (and adding) elements to see this happen first-hand.

Then we look at `scripts/views/directory.js`:

```
  events: {
    'click .controls .add': 'addForm',
    'submit .controls form': 'addSubmit'
  },
```

This code adds two different event listeners to our Directory View. Since the **Add New Contact** button simply toggles the visibility of the contact form, and the submitting of the contact form does the real work, we need the two separate events.

```
  addForm: function() {
    this.$('.controls form').show()
      .find('input.firstName').focus();
  },
```

The `addForm()` method makes the input form visible. We use a bit of jQuery function chaining to make it all happen. First, we select the form element located within the `.controls` element within our current View. We then run the jQuery `show()` method, which makes this element visible (in other words, it disables our inline CSS from the `<form>` tag). Next we look for the `firstName` input (which is the first input in the form and likely the one the user wants to type into) and we have jQuery focus on that element. This lets the user immediately begin typing after clicking on the button.

```
  addSubmit: function(event) {
    event.preventDefault();
    var $form = this.$('.controls form');
```

We begin this method by taking the event (the form submit event) and immediately telling the browser to stop whatever it was planning on doing via the `preventDefault()` method. If we didn't do this, the form would still submit and our Single Page Application would get blown away.

Next, we grab onto the form element and cache it into a local variable. The name of this variable is `$form`, and isn't necessarily a Backbone convention. By prefixing variables that are references to DOM elements that have been captured using jQuery with the `$` symbol, it is easier to look at them and know what type of data they contain. Backbone does follow this convention in some places though, such as the `$el` attribute on Views.

```
    var newContact = new App.Models.Contact({
      firstName: $('input.firstName', $form).val(),
      lastName: $('input.lastName', $form).val(),
      phoneNumber: $('input.phoneNumber', $form).val(),
      email: $('input.email', $form).val()
    });
```

Here we take all of the data from the form and pass it into a newly instantiated Contact model. Note that when we create this Model, we haven't actually put it into the Collection yet. It is just an orphaned Model for now.

```
    if (newContact.isValid()) {
      App.Contacts.add(newContact);
      $form.hide();
      $('input[type=text]', $form).val('').blur();
    } else {
      alert(newContact.validationError);
    }
```

This is the reason we took the raw data from the user and added it to a Model before checking it for errors. We will use the `isValid()` method provided to Models to check and see if the data is valid or not.

If the Model is found to be valid, we insert it into the Contacts collection, hide the form, and then remove focus from any of the input fields by using the `blur` method provided by jQuery. If we didn't do this, weird things could happen from having focus in there.

If the Model is not found to be valid, we display an error message to the user. This message is the result of the `isValid()` function executed earlier. Of course, if this were a real application, you'd want to display the message in a less intrusive manner, such as toggling a message with red text next to the form.

### Note

I left out something very important in the functionality of this application. We can add contacts and delete them, but we can't edit them! Using the knowledge you have obtained so far about Backbone, try to add the editing functionality to this app.

## URL-based routing

When a user navigates around a traditional Request / Response website, they fully expect the back and forward history navigation to work. With the stateless nature of the traditional website, this was very easy to implement. The browser needed only to keep track of the previously requested URLs and request them again when the user asks for them.

Single Page Applications built using JavaScript are stateful and will require programmer intervention to allow this appearance of functioning history. Luckily for us, Backbone provides mechanisms to make this process easier.

Users do expect the URL of the application to change when the content on the screen changes. Users would also like to share URLs with other people, as well as bookmark these URLs for later browsing. Typically, traditional web apps have a URL that points to a resource on the server, beginning with a forward slash. Every time a new page is requested, the server responds with an entirely new page based on that URL.

JavaScript, for the longest time, couldn't change the URL without redirecting the user to another page, except by changing the URL after the `#` symbol (often referred to as an anchor). However, there's a relatively new JavaScript API, that allows a URL to be changed without needing an anchor, as long as the user is kept within the current domain. This is called `pushState` and like many exciting web technologies, is only available in modern browsers and therefore may not meet the criteria for your project. Luckily, Backbone can work with either method, and provides a common interface for both.

When using `pushState`, as the user first requests a page deep within your application, the browser will make the full request to the server, as it has no way of knowing whether a URL is a normal server request URL or whether part of that URL was built using `pushState`. For these requests to work properly, the server-side application needs to be built in such a way to account for this. Since we're still building this application in our browser using the local filesystem, we will forego using `pushState` and use the traditional anchor-based URLs (the default for Backbone).

### Step 1 – creating a router script

Create a `scripts/router.js` file and add the following code to it:

```
App.Routers.Contact = Backbone.Router.extend({
  routes: {
    'contacts': 'contacts',
    'contacts/add': 'contactsAdd',
    'contacts/remove/:id': 'contactsRemove'
  },

  contacts: function() {
    App.Directory.render();
    App.Directory.addFormHide();
  },

  contactsAdd: function() {
    App.Directory.addForm();
  },

  contactsRemove: function(id) {
    App.Contacts.remove(id);
    App.Directory.addFormHide();
  }
});
```

### Step 2 – updating the document

Update `index.html` and add the following line after loading `scripts/main.js` so that we can get access to this new script file: 

```
  <script src="scripts/router.js"></script>
```

### Step 3 – updating the main script

Update `scripts/main.js` so that the main `App` object has two more keys such as the following (don't forget that trailing comma):

```
var App = {
  …,
  Routers: {},
  Browser: null
};
```

Also in `scripts/main.js`, add the following two lines at the end of our document-ready function:

```
  App.Browser = new App.Routers.Contact;
  Backbone.history.start();
```

### Step 4 – updating the Contacts view

Update `scripts/views/contact.js`, and add the following line of code to the beginning of the `remove()` method:

```
    App.Browser.navigate('contacts/remove/' + this.model.cid);
```

### Step 5 – updating the directory view

Update `scripts/views/directory.js`, and add the following line of code to the beginning of the `addForm()` method:

```
    App.Browser.navigate('contacts/add');
```

Also, add this new method to the directory view file after the existing `addForm()` method:

```
  addFormHide: function() {
    App.Browser.navigate('contacts');
    this.$('.controls form').hide();
  },
```

Be sure to add `addFormHide` to the `bindAll()` call in the `initialize()` method of this object:

```
_.bindAll(this, 'render', 'addForm', 'addSubmit', 'addFormHide');
```

Let's also do a quick refactor of the `addSubmit()` method. Replace the line of code that contains `$form.hide()` with the following line of code:

```
      this.addFormHide();
```

Once you've performed these steps, reload the web page. Perform some different actions on the app, such as adding new contacts, deleting contacts, and so on. Notice how the URL changes while you perform different actions.

Now, click on the button to add a new contact and, while the form is displayed, completely refresh the page. Or better yet, copy the URL, close your browser window, then open the URL in another fresh browser window. Notice how the add contact form is displayed while the page is loaded! While our application is fairly simple, this will allow sharing our URLs as well as bookmarking URLs.

Let us first look at `scripts/router.js`:

```
App.Routers.Contact = Backbone.Router.extend({
```

We begin by extending the built-in Backbone router object.

```
  routes: {
    'contacts': 'contacts',
    'contacts/add': 'contactsAdd',
    'contacts/remove/:id': 'contactsRemove'
  },
```

Here we've defined three different routes. The first route is executed when the URL simply states contacts. Since we're using anchor-based URLs, the URL will say `index.html#contacts`. Had this been using a web server, the URL would be `example.com/#contacts`. Had we been using a web server as well as the `pushState` URLs, the URL would be `example.com/contacts` and would resemble a traditional web page request.

The value of the router is the name of the method on our object to be executed (much as with our events object in the View objects).

The interesting one here is the third route. Notice the third URL parameter, `id`. This tells Backbone that we're expecting some sort of string in this part of the URL (basically any characters except for another forward slash). If you did want to include slashes, you can prefix the identifier with an asterisk, such as `*id`. The names you choose for identifiers in these dynamic routes don't matter much, as they are passed as arguments to the router methods in the order that they appear in. Name them something that you will remember, though.

```
  contacts: function() {
    App.Directory.render();
    App.Directory.addFormHide();
  },
```

The `contacts()` router method (executed when the URL matches `contacts`) is used for showing a listing of contacts. It will render our Directory again and perform a sort of page refresh for us. It will also hide the add form, if it were open. Think of this as the default state of the application. If the application performed several other functions, we would use this to bring the user back to the contacts listing.

```
  contactsAdd: function() {
    App.Directory.addForm();
  },
```

The `contactsAdd()` router method (executed when the URL matches `contacts/add`) is used for showing the **Add New Contact** form.

```
  contactsRemove: function(id) {
    App.Contacts.remove(id);
    App.Directory.addFormHide();
  }
```

The `contactsRemove()` router method (executed when the URL matches `contacts/remove/:id`) is used for performing a remove on the specified contact. The name of our first argument is `id`, and is coincidentally the same as the identifier used in our dynamic route. Either of these could have been named something else and the code would still execute as expected. This code executes the Contact models `remove()` method as expected, and hides the contact add form if it was open.

```
scripts/main.js:
var App = {
  …,
  Routers: {},
  Browser: null
};
```

The `Routers` object we added to the main `App` object will contain a listing of different routers we use in the application. If your application were to get much more complicated, it wouldn't be a bad idea to keep different components separated in this manner.

The `Browser` object will contain our instantiated Contact router. Currently the application is rather simple and having a single global pointer makes it convenient to work with.

We will now take a look at `scripts/views/contact.js`:

```
    App.Browser.navigate('contacts/remove/' + this.model.cid);
```

This line of code is executed when the remove button is clicked. It updates the URL so that it shows which item is being deleted. However, simply running this code does not cause the applicable route to be updated. If we wanted to do that, we would need to pass a second argument to `navigate()`, and it looks like this:

```
{trigger: true}
```

The reason we don't need to pass that second argument is because we're already going to be running the Model destroy code on the next line. However, if the document ever contains a link such as the following, it will trigger the router deletion:

```
<a href="#contacts/remove/c1">Delete First Contact</a>
```

We will also take a look at `scripts/views/directory.js`

```
    App.Browser.navigate('contacts/add');
```

This of course changes our URL for the Add Contact form. If someone only ever goes to our application for the purpose of quickly adding new contacts, this URL becomes very convenient for them.

```
  addFormHide: function() {
    App.Browser.navigate('contacts');
    this.$('.controls form').hide();
  },
```

Here we've refactored the code a little. Now that we've added the concept of a router, the need to toggle the visibility of the Add Contact form from elsewhere is important. We update the call in `addSubmit()` later to use this new function to complete the refactor.

Changing the URL anchor dynamically is good enough to allow the user to bookmark pages, share pages with friends, and navigate through history using their browser's forward and back buttons. However, if you want to get rid of the anchor symbol `#` (this simply looks nicer) you will need to look into a server-side solution to achieve this.

### Note

The method for this will be different depending on the backend server software you choose for your project. As a general rule, though, you'll want to execute the same page generation script and keep track of dynamic URL rules using regular expressions. Apache has `mod_rewrite`. Nginx has the HTTP location directive.

## Persisting data to a server

One of the biggest features of Backbone is how well it keeps data in synchronization between your backend server / database, and the frontend interface that your user is interacting with. Collections and Models are such a big part of Backbone that it would be a shame if we didn't have a backend to persist our data to!

This section of the book will have you create a simple **Node.js** based server in which we can save our contacts. There are two big reasons we're going to use Node for this project. The first is because programming is done in JavaScript, and you already know JavaScript! The second is that starting a server in Node for development is extremely simple. No need to spend hours tweaking configuration files and installing package dependencies.

### Step 1 – getting ready

Download and install Node for your operating system. You can download a Node package from the following URL:

[http://nodejs.org/download/](http://nodejs.org/download/)

Next, create a `server.js` file. This file should sit in the root of your project directory. Since the browser will never execute this code, it doesn't need to follow our convention of living inside of the `scripts` directory. In fact, it shouldn't follow the convention at all, otherwise one could get confused about the purpose of the script. You also will not need to load it using a `<script>` tag in our HTML file.

### Step 2 – creating a server script

Add the following code to `server.js`. I won't be explaining the code line-by-line, like with the other code we've written, as it is beyond the scope of this book:

```
var http = require('http');
var filesystem = require('fs');

// Our Contacts "Database"
var contacts = [{
  id: '1',
  firstName: 'Thomas',
  lastName: 'Hunter',
  phoneNumber: '9895135499',
  email: 'me@thomashunter.name'
},
{
  id: '2',
  firstName: 'Rupert',
  lastName: 'Styx',
  phoneNumber: '9895551212',
  email: 'rupertstyx@example.com'
}];

http.createServer(function (req, res) {
  console.log(req.method + ' ' + req.url);

  if (req.url === '/' && req.method === 'GET') {
    // Requesting HTML Document
    filesystem.readFile(__dirname + '/index.html',
      function(err, data) {
      if (err) throw err;
      res.end(data);
    });

    return;
  } else if (req.url.indexOf('/contacts') === 0) {
    // Backbone requesting some data
    res.writeHead(200, {
      'Content-Type': 'application/json'
    });

    if (req.method == 'GET') {
      // Backbone wants a list of all Contacts
      res.end(JSON.stringify(contacts));
    } else if (req.method == 'POST') {
      // Backbone is creating a new Contact
      var body = '';
      req.on('data', function(data) {
        body += data;
      });
      req.on('end', function() {
        var contact = JSON.parse(body);
        // Here we make up a unique ID
        contact.id = Math.random()
          .toString(36).substr(2);
        // The contact is stored in the DB
        contacts.push(contact);
        // Now we send the updated contact
        // (which has an ID) back to the
        // Backbone application
        res.end(JSON.stringify(contact));
        console.log("CONTACT ADD: ",
          contact);
      });
    } else if (req.method == 'DELETE') {
      // Backbone is deleting a Contact
      var id = req.url.split('/')[2];
      console.log("DELETE CONTACT: ", id);
      for (var index in contacts) {
        if (contacts[index].id === id) {
          // Delete Contact from DB
          contacts.splice(index, 1);
          break;
        }
      }
      res.end();
    }

    return;
  } else {
    // Browser probably requesting JavaScript
    var filename = __dirname + req.url;

    filesystem.lstat(filename,
        function(err, stats) {
      if (err || !stats.isFile()) {
        if (req.url != '/favicon.ico') {
          console.log("404: " + filename);
        }
        res.writeHead(404, {
          'Content-Type': 'text/plain'
        });
        res.write('404 Not Found\n');
        res.end();
        return;
      }

      filesystem.readFile(filename,
        function(err, data) {
        res.writeHead(200, {
          'Content-Type': 'text/javascript'
        });
        res.end(data);
      });
    });
    return;
  }

}).listen(1337);

console.log('http://localhost:1337/');
```

### Step 3 – updating the Contacts collection

Update `scripts/collections/contact.js`, and add the following attribute after the existing `model` attribute:

```
  url: '/contacts'
```

### Step 4 – updating the main script

Update `scripts/main.js`, and remove all the lines where we add the two contacts.

Also in `scripts/main.js`, add the following line of code at the end of the DOM ready code:

```
  App.Contacts.fetch();
```

### Step 5 – updating the router

Change the `contactsRemove()` method in `scripts/router.js` to resemble the following:

```
  contactsRemove: function(id) {
    var contact = App.Contacts.get(id);
    if (contact) {
      contact.destroy();
    }
    App.Directory.addFormHide();
  }
```

### Step 6 – updating the Contact view

Change the `remove()` method in `scripts/views/contact.js` to resemble the following:

```
  remove: function() {
    App.Browser.navigate('contacts/remove/' + this.model.get('id'));
    this.model.destroy();
  }
```

### Step 7 – updating the directory view

In `scripts/views/directory.js`, update the `addSubmit()` method by adding the following line of code after the line where we add the contact to the collection:

```
      newContact.save();
```

### Step 8 – running the server

Running this Node application should be as easy as opening up a Terminal window as shown in the following screenshot, typing "`node `" (note the space), dragging and dropping the `server.js` file onto the Terminal window, and pressing *Enter*.

In OS X and most Linux's, you can launch the Terminal application. In Windows, you can run the command prompt application.

![Step 8 – running the server](graphics/5668OS_04_04.jpg)

Once you've executed the Node server, you should see the message **http://localhost:1337/** displayed in your terminal. This is the URL that the server is listening on for incoming HTTP requests. Open that URL in your browser and you should see your application again! From now on, you'll want to use that URL to navigate to your application, instead of double-clicking on the `index.html` file directly.

You should see a listing of the two contacts that you've been seeing before, namely **Thomas Hunter** and **Rupert Styx**. Try adding some more contacts, and deleting contacts. But, this time, refresh the page in-between. This time they are saved on the server side, instead of being deleted every time you refresh!

One of the most noteworthy changes we've made in the Backbone application is the addition of `id` attributes on our Contact models. As soon as Backbone sees that `id` attribute, it knows how to refer to objects on the server. We also added some features for telling Backbone at what point to talk to the server for persisting and retrieving data.

Let us first have a look at `scripts/collections/contact.js`:

```
  url: '/contacts'
```

The `url` attribute on Collection objects is a Backbone convention for specifying the root URL point that represents that given type of data.

When Backbone wants to create a new contact, it will perform a POST operation on that location. When Backbone wants to get a list of all items in the collection, it will perform a GET operation on that location. When Backbone wants to remove a particular item, it will perform a DELETE operation on that location followed by a slash and a unique identifier. When Backbone wants to retrieve a particular item from the server, it will perform a GET operation on that location with the unique identifier. Finally, when Backbone wants to change an existing item, it will perform a PUT operation on that location with the unique identifier.

This is a very common way of using URLs to represent data on the server. This common way of interacting with resources is generally referred to as REST. Because of this, you could say that this server script provides a RESTful API for the frontend Backbone application to interact with.

Now we will look at `scripts/main.js`:

Most of the changes in this file are removing the code, which adds two default contacts since we would prefer the backend server to take care of these for us.

```
  App.Contacts.fetch();
```

This line of code tells Backbone to perform a fetch of the data. Remember from before that, since Backbone wants all the data from the collection, it is performing a GET operation at the URL /contacts.

Open your Network console for Chrome, and select what should be the last network request "contacts". This will show you exactly what the data being sent from Node to Backbone looks like. You should see an array of objects like the following screenshot:

![Step 8 – running the server](graphics/5668OS_04_05.jpg)

If this were an application built from the ground up using just jQuery, you probably would have written a `$.ajax()` call to perform this work, and iterate over each item by hand. Backbone actually uses the same `$.ajax()` call provided by jQuery, but does it all behind the scenes for us automatically!

Let us now have a look at `scripts/router.js`:

```
  contactsRemove: function(id) {
    var contact = App.Contacts.get(id);
    if (contact) {
      contact.destroy();
    }
    App.Directory.addFormHide();
  }
```

Previously, our code was running `App.Contacts.remove(id)`, where `id` was the collection ID. This is something that Backbone automatically adds to each model in a collection for keeping track of it internally. But now that we've updated the application to make use of real ID values, things get a little more complicated.

As we want the deletion of the Contact to be persisted to the server, we need to use `model.destroy()`, instead of `collection.remove()`. First, we attempt to get the contact from the collection and assign it to the variable contact. The user could attempt to delete a contact that doesn't exist, and we'd get an error. We then check to see if the model came back and, if it did, we run `destroy()`. This sends the DELETE command back to the server.

Let us take a look now at `scripts/views/contact.js`:

```
  remove: function() {
    App.Browser.navigate('contacts/remove/' + this.model.get('id'));
    this.model.destroy();
  }
```

As mentioned in the previous section, we now want to use the real ID of the models instead of the Collection ID. Instead of grabbing `model.cid`, we now use `model.get('id')` and pass that along to the URL for the router to handle.

Finally let us take a look at `scripts/views/directory.js`

```
      newContact.save();
```

The preceding line of code specifically tells Backbone to take this newly created contact and save it to the server. Just adding a model to a Collection isn't enough for Backbone to persist that collection to the server, unfortunately. It does, however, give us more control over the application.

If you inspect the network traffic for this request, you will see it perform the POST operation on the server. However, it will also respond with a complete document. Backbone then parses this document, looks for the `id` attribute, and then updates the local model. Typically, we want the database to come up with `id` values for us, and Backbone honors this for us.

### Note

If you were to kill the Node application (either by pressing *Ctrl* + *C* or closing the window), the contact database will be deleted forever. The reason this happens is that Node is simply storing the data in a variable in memory, which is cleared when the program exits.

If you want to save data forever, look into using a database system, such as MySQL, MongoDB, or Redis. There are many options out there for data storage. MongoDB, for example, works great with Node, and luckily for us JavaScript developers, uses JSON for representing objects.

More links for you to learn about Backbone.

## Official sites

*   Backbone documentation: [http://backbonejs.org/](http://backbonejs.org/)
*   Backbone GitHub: [http://github.com/documentcloud/backbone/](http://github.com/documentcloud/backbone/)
*   Underscore Documentation: [http://underscorejs.org/](http://underscorejs.org/)
*   jQuery Documentation: [http://api.jquery.com/](http://api.jquery.com/)
*   Node.js Documentation: [http://nodejs.org/api/](http://nodejs.org/api/)
*   The code for the examples in this book can be obtained from [https://github.com/tlhunter/backbone-book](https://github.com/tlhunter/backbone-book)

## Community

*   Freenode IRC: #documentcloud
*   Backbone issues: [http://github.com/documentcloud/backbone/issues](http://github.com/documentcloud/backbone/issues)
*   Mailing list: [https://groups.google.com/forum/#!forum/backbonejs](https://groups.google.com/forum/#!forum/backbonejs)

## Twitter

*   Follow the creators of Backbone.js at `@documentcloud`
*   Follow the author of this book at `@tlhunter`
*   For more open source information, follow Packt at `@packtopensource`