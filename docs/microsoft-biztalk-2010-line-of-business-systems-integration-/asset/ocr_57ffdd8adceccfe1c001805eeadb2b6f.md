![](bbox=[0.308, 0.144, 0.692, 0.279])

7. Next, we need to create an endpoint from the Endpoints form. The endpoint links together our local endpoint, BizTalk channel, BizTalk service account, and active Dynamics AX actions. Again, each company will require a local endpoint and thus we'll use the same naming conversion.

8. We'll start by typing in the Endpoint ID to CDFEndpoint and select the Local endpoint ID to CDFLocalEndpoint.

![](bbox=[0.212, 0.416, 0.788, 0.581])

9. On the General tab, specify the Outbound channel ID to BizTalkChannelID, which we created above. This will also allow asynchronous outbound messages to be retrieved from Dynamics AX 2009 using the BizTalk adapter.