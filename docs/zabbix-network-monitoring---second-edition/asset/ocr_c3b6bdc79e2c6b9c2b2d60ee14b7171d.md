While this view is very nice, it isn't too flexible. Can we have direct access to underlying data, perhaps? Let's visit **Monitoring | Latest data** to find out. Choose **Webpages** in the **Host groups** field, and click on **Filter**. Items within the **Webpage** application will show up. Take a look at the data – all of the collected values are accessible as individual items, including download **SPEED**, **RESPONSE TIME**, **RESPONSE CODE**, and even the last error message per scenario. We may reuse these items, thus creating whatever graphs we please – maybe we want a pie chart of response times for each step or a non-stacked graph of download speeds. Of course, as with all items, we get simple graphs without any additional configuration.

| NAME ▲ | LAST CHECK | LAST VALUE |
|---|---|---|
| Webpage (9 Items) | | |
| Download speed for scenario "Main page". | 2016-05-13 01:16:37 | 28.78 KBps |
| Download speed for step "First page" of scenario "Main page". | 2016-05-13 01:16:37 | 3.16 KBps |
| Download speed for step "Traces" of scenario "Main page". | 2016-05-13 01:16:37 | 54.41 KBps |
| Failed step of scenario "Main page". | 2016-05-13 01:16:37 | 0 |
| Last error message of scenario "Main page". | | |
| Response code for step "First page" of scenario "Main page". | 2016-05-13 01:16:37 | 200 |
| Response code for step "Traces" of scenario "Main page". | 2016-05-13 01:16:37 | 200 |
| Response time for step "First page" of scenario "Main page". | 2016-05-13 01:16:37 | 4s 257.7ms |
| Response time for step "Traces" of scenario "Main page". | 2016-05-13 01:16:37 | 439.2ms |

There's also a failed step item, which returns 0 if none of the steps failed. As that value is 0 when everything is fine, we can check for this value not being 0 in a trigger, and alert based on that.

> While we could use value mapping to show **Success** when the failed step is 0, we would have to add a value map entry for every step number — value mapping does not support ranges or default values yet.