An approach that may be beneficial to both the SAP Developer and BizTalk developer is to have an SAP resource generate an initial IDOC and have BizTalk process the IDOC. If the BizTalk developer needs to perform additional tests, or even load tests, against his interface he can do this by using an SAP transaction called "WE19". This transaction will reprocess both incoming and outgoing IDOCs.

If we want to resend an IDOC to BizTalk, we need to know the IDOC number for the original IDOC and then we can resubmit the message form SAP to BizTalk. In order to get the IDOC number, we will use a transaction called "WE02". Most SAP developers will have access to these two transactions so it should not be too much of a stretch for BizTalk Developers to get the same access for these transactions. Other than trying to process too many messages, in load test scenarios, there is not much damage that a user can do with these two transactions.

Let's now walkthrough the process of browsing WE02, getting an IDOC number, and then using that IDOC number in WE19 to submit an IDOC to BizTalk.

+   1. Once you have logged into the SAP GUI, type WE02 into the navigation text box and hit *Enter*.
+   2. In WE02, we have the ability to restrict the time frame that we would like to query upon. If your organization processes a lot of IDOCs you will want to ensure this query window is small. Once we have specified our query timeframe, we can click on the **Execute** button, which is represented as a Clock with a green check mark.

![](bbox=[0.255, 0.53, 0.745, 0.825])