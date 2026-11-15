
*   We need to store billions of records
*   Each object we are going to store is small (less than 1K)
*   There are no relationships between records, except if we want to store which user created what URL
*   Our service is read-heavy

We would need two tables, one to store the URL and one to store the count of the link.

The following represents the `URL` table:

| `_id` |
| --- |
| `long_url` |
| `clicked` |
| `created_date` |

The following represents the `Counter` table:

| `_id` |
| --- |
| `Seq` |

# Encoding URL algorithms

This is the interesting part where the actual URL encoding and decoding happens.

Before we jump into the code development, let's explore the basic theory behind URL shortening. Our requirement is as follows:

Given a long URL such as `http://stackoverflow.com/questions/tagged/node.js`, generate a unique URL that is shorter, such as `localhost:3000/3Ys`.

First, let's look at actually generating a unique key from our original URL, in this case 3 years, which is at the end of our localhost URL.

To satisfy the uniqueness requirement, we can use a special class of functions called bijective ([https://en.wikipedia.org/wiki/Bijection](https://en.wikipedia.org/wiki/Bijection)) functions, which guarantee a 1-to-1 mapping. For explanation purposes, a bijective function basically says:

"A decoded URL is mapped to exactly one key, and a key is mapped to exactly one decoded URL"

We are going to use a base58 algorithm to fulfil our URL shortening and decoding task.

We are going to convert a unique integer ID (which is in base10) to its equivalent in base58. The base58 alphabet we will be using is as follows:

```
123456789abcdefghijkmnopqrstuvwxyzABCDEFGHJKLMNPQRSTUVWXYZ
```

It is just the numbers 1-9, a-z, and A-Z, giving us a total of 58 characters, hence the 58 in base58. We are not including 0, l, or O to avoid confusion when sharing the URL over the phone or copying it manually.

Now, to put it all together to generate a shorter URL, our solution will be as follows:

1.  Create a global auto incremented integer
2.  Every time a new URL is shortened and added to our database, we'll increment that global number (base 10) and use it as our unique ID for that entry in the DB
3.  Then, we will base58 encode that unique ID to generate a unique, shorter URL

For example, an entry with the unique ID 100022 (base 10) will result in a base58 encoding of 3Ys. So if you store a lot of URLs, say 100,000,000, they would generate a shortened hash:

```
9QwvW
```

Let's develop the code!

# Node app development

So, we have everything set out; let's build the app.

Let's create our `package.json` file.

Always use the `npm init --y` command to generate boilerplate `package.json` file.

Here are the packages we require:

*   `body-parser`
*   `express`
*   `mongoose`

Let's install them. Run this command under your project directory to install the dependencies:

```
npm install --save body-parser express mongoose
```

This is what my `package.json` looks like:

```
{
  "name": "url-shortener",
  "version": "1.0.0",
  "description": "A NodeJS + Express + MongoDB based URL shortener",
  "main": "app.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "shahid",
  "license": "MIT",
  "dependencies": {
    "body-parser": "^1.14.1",
    "express": "^4.13.3",
    "mongoose": "4.2.9"
  }
}
```

Let's create the configuration file. Name it `config.js` and place the code, shown as follows:

```
var config = {};
config.db = {};
config.webhost = 'http://localhost:3000/';
config.db.uri = 'copy the uri from Azure portal';
config.db.host = 'packt-mongodb-2.documents.azure.com';
config.db.name = 'url_shortener';
module.exports = config;
```

Copy the URI from the Azure portal screen. Refer to the following screenshot for reference:

![](assets/93a8f365-1b81-4999-ab51-4c3d35829793.png)

We have done the configuration. Let's write the code to connect to our MongoDB database of Cosmos.

Create a file called `connection.js` and place the following code in it:

```
const mongoose = require('mongoose');
const config = require('../config.js');
mongoose.connect(config.db.uri);

module.exports = mongoose.connection;
```

This code will return the connection object to the caller.

Next, we will create database schema codebase which is to ensure the MongoDB schema and data indexing.

Here is the codebase for the same. Place this in the file named `url.js`:

```
var mongoose = require('mongoose');
var connection = require('./connection');
var Schema = mongoose.Schema;

connection.once('open', function() {
  console.log("Connected....");
});

var CounterSchema = Schema({
    _id: {type: String, required: true},
    seq: { type: Number, default: 0 }
});

var counter = mongoose.model('counter', CounterSchema);

// create a schema for our links
var urlSchema = new Schema({
  _id: {type: Number, index: true},
  long_url: String,
  clicked: {type: Number, default:0 ,index: true},
  created_at: Date
});

urlSchema.pre('save', function(next){
  var doc = this;
  counter.findByIdAndUpdate({_id: 'url_count'}, {$inc: {seq: 1}}, function(error, counter) {
      if (error)
          return next(error);
          console.log(counter);
      doc.created_at = new Date();
      doc._id = counter.seq;
      next();
  });
});

var Url = mongoose.model('Url', urlSchema);

module.exports = Url;
```

As you can see in the preceding code, we are using our connection object and printing the status of the connection on the console.

As we discussed in the database section, we have created two schemas. One is to maintain the global counter and the other is to maintain the actual link.

We are using the pre-function event of Mongoose to basically get the latest count of the URL value from the counter collection. This is to make sure that we never generate duplicate short URL.

At the end, we are exporting the database object.

Next, we will develop our application logic, that is, server code to handle the request and response from the user and call functions which we developed.

Here is the code base for the server. We are using the express node module to create server routes:
