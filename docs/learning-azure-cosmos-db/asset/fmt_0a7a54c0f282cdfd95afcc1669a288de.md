
# Building an Email Sender with Node.js

Now, what if we replace the path to an actual image with the path to our custom API? When the API gets hit, we know someone has opened up an email. We can include additional information in the URL, such as subscriber ID, email ID, and so on, allowing us to gather more information.

Pretty amazing, right?

Now, we already know how to create a link. We just made an URL shortener in the last chapter. Whenever the shortener decodes any email, you know that the link has been clicked.

Okay, enough of the producer. Once the producer generates the emails, it pushes those messages into the queue and its job is done.

Now, the queue stores those messages and delivers them to its consumers. Here comes the fun part.

In the diagram, I have shown one consumer receiving the message from the queue. In reality, you can send an email to multiple consumers at the same time.

*I know what you're thinking; none of the consumers will get the same message.*

So let's say, you have 10 consumers at the queue end, you can configure the number of the messages they can receive at one particular time. You can send, say, 100 messages at a time.

So, if we send 100 messages to 10 consumers at a time, that is 1,000 messages at a time. Consumers will then push those 1,000 messages to the SMTP server to deliver them to your inboxes. This process goes on until the queue is empty, which it never will be in a production grade system.

I can't say with certainty, but this is, or is similar, to what every major piece email marketing software is using.

## Project Setup

Okay, code time. Let's build a simple yet efficient email sender using Node.js.

First, create a new directory and run this command inside the directory to create your fresh Node.js project:

```
mkdir dirname && npm init --y
```

Replace `dirname` with your folder name.

## Installing Dependencies

Once the project is created, you need to install the dependencies of that project. We are going to send emails, so of course we need a module which can handle that. We also need to handle the asynchronous flow of the node, so install a module for that too.

Run this command to install these two dependencies:

```
npm install --save nodemailer async
```

As the name suggests, `nodemailer` is used for sending out an email and `async` will be used to handle flow control.

## Email Sender Implementation

Let's jump in and code our server.

Here is the code for the emailer. I will explain it function by function:

```
const async = require("async");
const nodemailer = require("nodemailer");
// can be your email list from DB
const listofemails = ["rwtc66@gmail.com","shahid@codeforgeek.com"];
const emailContent = "Hello World, this is Shahid Shaikh! <br>";

var success_email = [];
var failure_email = [];

class Mailer {
    constructor() {
      this.transporter = nodemailer.createTransport("SMTP",{
            host: 'smtp.gmail.com',
            port: 587,
            auth: {
              user: '',
              pass: ''
           },
            tls: {rejectUnauthorized: false},
            debug:true
          });
      this.invokeOperation();
    }

    /* Invoking email sending operation at once */

    invokeOperation() {
        var self = this;
        async.each(listofemails,self.sendEmail,function(){
            console.log(success_email);
            console.log(failure_email);
        });
    }

    sendEmail(Email,callback) {
        console.log("Sending email to " + Email);
        var self = this;
        self.status = false;
        async.waterfall([
            function(callback) {
                var mailOptions = {
                    from: 'shahid@codeforgeek.com',
                    to: Email,
                    subject: 'Hi ! This is from Async Script',
                    text: emailContent + "<img src='http://domain.com/api/track/?email='+to>"
                };
                transporter.sendMail(mailOptions, function(error, info) {
                    if(error) {
                        console.log(error)
                        failure_email.push(Email);
                    } else {
                        self.status = true;
                        success_email.push(Email);
                    }
                    callback(null,self.status,Email);
                });
            },
            function(statusCode,Email,callback) {
                    console.log("Will update DB here for " + Email + "With " + statusCode);
                    callback();
            }
            ],function(){
                //When everything is done return back to caller.
                callback();
        });
    }
}

new Mailer(); //lets begin
```

## Code Explanation

First, we declared our dependencies and also declared a few variables. We stored an email list in an array, which can be replaced with emails from the database or an API call.

We also declared two separate arrays to hold the successfully sent emails and rejected emails, respectively.

Next, we created a class in the constructor, creating our SMTP transport, using it to send a huge number of emails.

We created a Gmail SMTP and, by using Google email credentials, we are going to send the emails.

Google SMTP is just for demonstration purposes; you must use a fully-fledged SMTP server in production.

After the creation of the SMTP transport, we invoke our operation and, using that, we are going to perform a parallel iteration to the emails. The `async.each` function would call the function a number of times and then email count.

Now, hold on: what if the email count is in the millions? Does it create a million parallel processes?

Well yes, but it should not.

So, we can add a limit—say, 1,000 parallel processes—in the `async.each` function; iteration would then be limited.

Finally, in the `sendEmail` function, we send the email to the destination coming from the list and store the result of that action in the respected array depending upon the condition.

This is by far the simplest email sender you will ever see. Trust me on that!

We invoke the program using the traditional `new` operator. You can extend this program to meet your production system needs with a few changes here and there, but it will work.

Now, regarding tracking, you will see in the email sending part that we are appending an image. Here is the code for the reference:

```
emailContent + "<img src='http://domain.com/api/track/?email='+to>"
```

Here, the `src` attribute of the image is our API URL. You can replace the domain name with your app. We add the email ID to the URL to identify which user has done it.

Let's go ahead and see how we can track those emails.

# Tracking Emails and Storing Tracking Data

So far, we have discussed the architecture of our email sender and how we can build a scalable system which can send emails in large volumes.

Now, it's time to track those emails. Let's build and code a program that can track the email open rate, location, IP, device, that is, computer, laptop or phone, and agent, that is, browser.

We are also going to insert that data in the MongoDB database or Cosmos DB.

In the last section, we added the tracking code, which was an image tag using our API call as its source, into the email. Our API call is going to return the image.

Let's develop a sample code which will do the same thing.

Initiate a new Node.js project and install the following dependency:

```
npm i --S express
```

Here is the sample code of the API:
