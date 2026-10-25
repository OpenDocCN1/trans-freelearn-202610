Knowing that the data we receive may not be authentic, why do we still need client-side validation? Simple. We need better user experience, and we cannot simply ignore a mechanism that will provide us with a mechanism to achieve it. Not only authenticity, but also if somebody disables the JavaScript in their client, then we may be processing unsafe data (not validated) received from the client.

The JavaScript could be disabled using the browser settings. The following screenshot shows how that can be performed using Chrome settings (Content | JavaScript):

![](bbox=[0.105, 0.28, 0.895, 0.6])