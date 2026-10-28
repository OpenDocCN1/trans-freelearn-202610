Finally, we have the actual guts of the program:

```
using System.ComponentModel.Composition;
using Fisharoo.BusinessLogic.Interfaces;
using Fisharoo.Common;

namespace Fisharoo.MailQueueProcessor
{
    public class Program
    {
        [Import]
        private static IEmailService _emailService;

        static void Main(string[] args)
        {
            MEFManager.Compose(null);
            _emailService.ProcessEmails();
        }
    }
}
```

As we did most of our work in the DBMailService, we have very little work left to do to actually process our email queue. We import an interface IEmailService, compose it, and call the ProcessEmails() method. We just wanted to show you that this could be done trivially. This means that the entire application that processes the email in our queue is just two lines of code!

# # Alerts

Alerts are notifications sent between users of the site. The question you may ask is how will this impact the performance of the site? As the community grows with more and more people and they become friends with each other, they become members of the groups, write blogs, access forums, etc. the amount of alert messages flowing in the system increases. This means rapid growth in data that needs to be stored and displayed to each user.

We will talk about database optimization below, where we address data file sizes. In case of alerts, while we need to handle the growth of the alert messages, we will also need to look at providing means to delete alerts that are no longer useful. This will help ensure to keep the data growth in check. We can further automatically purge data that is older than a certain date as needed. Another change we will do is restricting the alerts to be sent to only friends. There is no need to add an alert to self as the user already knows what activity he/ she has done on the site.