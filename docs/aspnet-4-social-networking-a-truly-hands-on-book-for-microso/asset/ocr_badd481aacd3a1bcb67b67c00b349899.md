# Scaling Up

To fake this process, we created a class called MyMailMessage with a subclass MailAddress. This encompasses all the features that we need to be represented in our emails at this point. We are avoiding the headers, attachments, and so on. At this point, we only care about the sender, the receivers, subject, body, and a few other things.

```
```csharp
//Fisharoo/BusinessLogic/MailMessageService.cs
[Serializable]
public class MyMailMessage
{
    public MailAddress[] Bcc { get; set; }
    //refer to actual file for complete code
    public MailAddress Sender { get; set; }
    public string Subject { get; set; }
    [Serializable]
    public class MailAddress
    {
        public string Address { get; set; }
        public string DisplayName { get; set; }
    }
}
```
```

Note that it is marked as **Serializable**!

With this in place, we were then able to create a few methods that effectively shuffle data in and out of the MailMessage class into and out of our new MyMailMessage object.

```
```csharp
//Fisharoo/BusinessLogic/MailMessageService.cs
private static MailAddressCollection ConvertMyMailAddressesToMailAddresses(List<MyMailMessage.MailAddress> MyMailAddresses)
{
    MailAddressCollection mac = new MailAddressCollection();
    foreach(var a in MyMailAddresses)
    {
        mac.Add(ConvertMyMailAddressToMailAddress(a));
    }
    return mac;
}
private static MailAddress ConvertMyMailAddressToMailAddress(MyMailMessage.MailAddress MyMailAddress)
{
    MailAddress ma = null;
```
```