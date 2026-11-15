
Collections represent database tables, and models represent individual rows within that table. With our application, we're going to be storing a bunch of people's contact information in our address book. We will have a `contacts` collection for all of our contacts and a different `contact` model for each person in our collection.

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

### Examining `scripts/main.js`

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

To see what the object looks like, open up your `Console`, simply type `tom`, and press `Enter`. You can then expand some of the attributes to see what the object looks like, as shown in the following screenshot:

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
