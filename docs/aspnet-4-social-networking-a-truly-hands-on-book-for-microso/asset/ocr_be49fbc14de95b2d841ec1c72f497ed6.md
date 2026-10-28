This class, like the others, uses MEF so that it can be stubbed out for testing later. Currently there are two methods—one an example, and another handling redirection. Let's look at the Redirect() method. It takes a path parameter and then uses the HttpContext object to redirect the user to the appropriate location. An example is the GoToHomePage() method. It asks the Redirect() method to send the user to the homepage. If the homepage of the application changes later, we need to edit only this method.

Of course, this class can be expanded with as many new methods as needed to redirect for any purpose. We can extend this object to be a bit more versatile too. We can also perform all sorts of logic inside these methods prior to using the redirection, obviously without degrading the overall design and where actually required.

# # Email

Sending emails is one task that every website has to be capable of. How many emails you plan to send should certainly determine how you go about sending that email. As we do not yet know how many emails we plan to send, we will initially rely upon the tools that are provided in the .NET framework to send our email. For brevity, we will look at the specific methods from this class here.

As with all our objects, we will have the MEF Export attribute in place that makes this a Pluggable class. The class itself inherits from our IEmail interface. We will have a couple of constants declared—one for the website's receiving the email account and another for the website's sending the email account. (We could have used one variable for both, but a little flexibility never hurt anyone!) We then jump into our first method:

```
public void SendEmail(string From, string Subject, string Message)
{
    MailMessage mm = new
    MailMessage(From,TO_EMAIL_ADDRESS);
    mm.Subject = Subject;
    mm.Body = Message;
    Send(mm);
}
```

This method is one of the various overrides for the SendEmail() method. This one is a bit different from the others in that it is used for the site to send email to another site rather than to a user. The user of this method will provide the email address that the message is from, the subject and the message. This method would be used in a 'Contact Us' page or a similar mail form. At the bottom of this method, you will see a Send() method call. This method spins up an SmtpClient and sends the email message as do each of the other SendEmail() methods.