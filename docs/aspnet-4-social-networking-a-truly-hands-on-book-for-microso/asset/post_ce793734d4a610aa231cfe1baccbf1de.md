
## Configuration

We have demonstrated three caching solutions: one uses System.Web.Caching (check Cache.cs in the code), another with AppFabric Caching, and the last one using MemCached. For this we have built one interface ICache and built a specific implementation wrapper Cache.cs, AppFabricCache.cs, and MemcachedCache.cs for each of the solutions respectively in the Components project.

In the real world you will need only one solution; whereas we have shown three so that you can build an understanding with each one. When you want to actually run the project you should use just one caching solution and exclude the other two caching class implementations from the Components project because failure to do so will have MEF raise an exception at runtime. This happens because MEF will find three matching imports for the ICache interface (from the three class implementations) and the composition container will not know which one to use.

However, MEF also provides a solution to such a problem by providing the ImportMany attribute, which can match multiple Import interfaces to one Export interface. Alternatively, one can mention the priority of matching Import in the [Metadata] attribute and the highest priority Import will get assigned. However, to keep things simple you can just exclude the other two class files here.

### Where do I start?

The easiest way to know where to put caching is to analyze the performance of your site and find the slowest parts. There are at times sections of code that may benefit from a tweak to squeeze out better performance. But frequently, there are highly optimized sections of code that just can't run any faster. These are the areas that may benefit from a cache wrapper. To get major gains from your caching you don't necessarily need to stow your items away in the cache for days on end. You can see gains by caching something for only a few seconds. The key is how frequently your data changes, and how frequently that data is accessed by your users. If the data changes constantly and it is accessed constantly, huge gains can be made from seconds of caching. If the data doesn't change much you can put it into the cache and remove it only when your data changes.

## Email

How can sending email make the site slow? If you think about the normal emailing process, you will find that the web server has to, at the very least, connect to an SMTP server. This connection can take time. If you need to send hundreds of emails, then you need to possibly make the same amount of connections to an SMTP server.

To reduce this slowness, we will build a system that allows us to stuff our emails into a queue and send the email later from another system. This will greatly improve our users' experience as they will not have to wait for the web server to make a connection to SMTP and send an email. We will instead stuff the email into our queue and assume that the email will be delivered from another system. The mails will have some delay before they are delivered, but for our community site requirement, we don't expect the users will need real time communication anyway.

To this point we have discussed speeding up the site from a hardware point of view, and a caching point of view. We are sure that it didn't take you too much convincing that all these are wise places to look for eking that last bit of performance out of your site. Now we are going to look at speeding up the site from an infrastructure point of view. Specifically, we are going to look at how to handle our email processing in such a way that we will reduce the number of network connections required to be made by the website directly.

> The concept of queuing up system-to-system communications is not a new one. This can be done with almost any communication that does not need real-time feedback. Any asynchronous communication is a prime candidate. And you don't necessarily need to employ a database as your queue. You can use MSMQ, the file system, or any other responsive local or semi-local resource. The concepts are mostly the same even though the technologies are vastly different.

Specifically, any time your website needs to connect to an external server — say FTP, HTTP or SMTP — the site takes a major hit in trying to establish a connection, and once connected, to actually perform its work with the third-party resource. Any time we can come up with a way to remove this connection, our users will be much happier. From the users' point of view, they want to create a message, click a button, and get feedback on whether their message was sent. We can simulate that by allowing them to create a message, stuff it in a database, and then tell the user that the message was sent. The connection to a database for a transaction is much quicker than the connection to the SMTP server. So we will store our messages in the database and have another application process outbound email at a later time.

### Creating services to send email

We currently have an Email class which handles the creation of email, the actual connectivity to the infrastructure, and finally the sending of the email. You will agree that this particular class is way overtasked. For this reason, we will move out the infrastructure and sending of an email to an EmailService.

### Scaling Up

This service will have one method—Send()—to begin with. This method will be responsible for taking in a MailMessage, connecting to the mail server with a SmtpClient, and finally sending the email.

```csharp
//Fisharoo/BusinessLogic/EmailService.cs
public void Send(MailMessage Message)
{
    Message.Subject = _configuration.SiteName + " - " +
                      Message.Subject;
    SmtpClient smtp = new SmtpClient();
    smtp.Send(Message);
}
```

We will then go into the Email class and modify the implementation of the Send() method to invoke the EmailService:

```csharp
//Fisharoo/Components/Email.cs
private void Send(MailMessage Message)
{
    _emailService.Send(Message);
}
```

Now that our infrastructure is separated out a bit, we can address the need for storing the email in some form of a queue rather than sending the email directly. To do this, we will start by first addressing our queue needs. We will use a queue in the database.

> We could just as easily use an MSMQ or the file system with some XML files. Rather than adding something new to the project we figured we would build off our existing framework!

### The database

We need to build three tables. Each table will have exactly the same structure, but a different purpose. We will have a receiving table that will take in new emails. This table can't be interrupted. We want to be able to receive new emails while also processing the existing emails. To do this we will need to have a working table where we will put all the emails that need to be processed. And finally, as we have a log of all the communications that we have sent, we will also have a history table. This table will hold all the emails that we have processed over time.

Our tables will hold serialized email. This means that we will actually have the email, as sent, in its entirety. This means that we could easily re-serialize a batch of emails to be resent if we ever needed to. The only thing that is different about each of these tables is their names!

In order to process email that is received, we may have to go outside of what LINQ was designed for. For this reason, we will create a couple of stored procedures to help us out. We will have two procedures—one to essentially put the data from receiving into the working table, and the other to put the working data into the history table. This is just the idea, not the actual implementation!

```sql
create procedure [dbo].[pr_MailQueue_SwapReceivingAndWorking_GetWorking]
as
begin tran
--rename working to temp
execute sp_rename
    @objname = 'MailQueue_Working',
    @newname = 'MailQueue_Temp'
--rename receiving to working
execute sp_rename
    @objname = 'MailQueue_Receiving',
    @newname = 'MailQueue_Working'
--rename temp to receiving
execute sp_rename
    @objname = 'MailQueue_Temp',
    @newname = 'MailQueue_Receiving'
select * from MailQueue_Working
commit tran
```

This procedure moves the data from one table to another in a manner that is more efficient than actually moving the data. We start by renaming the working table (an empty table at this point) to MailQueue_Temp. We then rename the receiving table (a full table) to MailQueue_Working. And finally, we rename the temp table to MailQueue_Receiving so that we can continue to collect new emails. With this complete, we then return all the records in the working table. If this process is not run frequently enough, you might find that there is more data in this table than you can process in a single chunk. For that reason, you either need to process it more often, or modify the selection to select pages of data. Our implementation will assume that you process your queue frequently enough so that the processor is never overloaded.

The next procedure, pr_MailQueue_MoveWorkingToHistory, cleans up after our email processor has completed its work.

```sql
create procedure [dbo].[pr_MailQueue_MoveWorkingToHistory]
as
begin tran
```
