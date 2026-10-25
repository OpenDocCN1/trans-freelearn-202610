Note how the credit card expiry field is left incomplete. The Firebug window shows the DWR requests, and each time a character is entered in the field, the `checkExpiryFormat()` method is called. Note that, in this case, calling the server each time a character is entered introduces a lot of network traffic and overhead to the server code. However, in this sample, an illustrative example is worth the extra overhead on the server. Perhaps also in other cases, overhead may be considered secondary to good user experience and immediate feedback to the user.

When the fields are correct and the order is submitted, order is sent to the order processing system. After the order is completed, the user gets the confirmation for a successful order as shown in the following screenshot:

![](bbox=[0.12, 0.31, 0.88, 0.73])

Of course, in real life, the user would get a receipt for the order instead of the green **Order submitted** text.