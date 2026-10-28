```
```csharp
if(MyMailAddress != null && MyMailAddress.Address != null &&
   MyMailAddress.DisplayName != null)
    ma = new MailAddress(MyMailAddress.Address,
                         MyMailAddress.DisplayName);
return ma;
}
```
```

And finally, we have the Serialize() and Deserialize() methods that operate on the MailMessage with the helper methods.

As a great side effect of this class, we were also able to create the SerializeEncrypted() and DeserializeEncrypted() methods that take the returned string from the Serialize() method and wash them with our Cryptography.Encrypt() and Cryptography.Decrypt() methods respectively.

# # Connecting the new DBMailQueueService

Now that we have this new service up and running and ready to be tested, we hook it into our existing system without changing much of our existing code using MEF approach.

# ## The queue

Now we are actually able to collect email from our site to be deposited into our MailQueue. Since the implementation of this new feature is transparent to the site, all areas that send email will now deposit them into our queue.

> > Keep in mind that you have the option of encrypting emails that are to be stored in the queue. This might be a good idea considering that you have emails that go out with users' passwords in them. Just something to think about!

# ## Processing the queue

With a couple of emails in our queue, we can now focus on building a sample processor. We will build a quick console application to demonstrate this. Technically, you could take the console application and hook it up to your windows server as a scheduled task. But for production work, we suggest that you convert this console application to a Windows service, which is vastly more reliable (just not as easy to show demos of!).