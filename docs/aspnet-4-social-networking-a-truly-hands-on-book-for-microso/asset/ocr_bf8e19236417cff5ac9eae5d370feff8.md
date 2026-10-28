```
{
    using (FisharooDataContext dc = conn.GetContext())
    {
        dc.MailQueue_Receivings.InsertOnSubmit(MailQueue);
        dc.SubmitChanges();
    }
}
public List<pr_MailQueue_SwapReceivingAndWorking_GetWorkingResult>
    GetMailQueueToProcess()
{
    List<pr_MailQueue_SwapReceivingAndWorking_GetWorkingResult>
        results = new List<pr_MailQueue_SwapReceivingAndWorking_GetWorkingResult>();
    using (FisharooDataContext dc = conn.GetContext())
    {
        results =
        dc.pr_MailQueue_SwapReceivingAndWorking_GetWorking().ToList();
    }
    return results;
}
public void MoveMailQueueWorkingToHistory()
{
    using (FisharooDataContext dc = conn.GetContext())
    {
        dc.pr_MailQueue_MoveWorkingToHistory();
    }
}
```

# Services

With the repository and database out of the way, we need to create a new service. We already have an EmailService that directly sends email through an SMTP server. But what we need is a new service that can be swapped in place of the EmailService—the DBMailService. This service will need to conform to our IEmailService interface so that none of our application will need to know anything about how the email is sent.

Recall that our EmailService only had one method—Send(). We will have a Send() method that will deposit email into the new EmailRepository. But our DBMailService will also need a way for our email processor to process emails. So we need a second method of ProcessEmails().

```
//Fisharoo/BusinessLogic/EmailService.cs
public void Send(MailMessage Message)
{
```