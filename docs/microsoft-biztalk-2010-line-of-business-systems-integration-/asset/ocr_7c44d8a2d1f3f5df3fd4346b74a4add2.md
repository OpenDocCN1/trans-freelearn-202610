This step is where we can see that our naming conversion in the AIF setup is very important. The AIF metadata in our message determines which **SourceEndpoint** and **DestinationEndpoint** (**Endpoint** and **Localendpoint** tables in the AIF setup respectively) this message is intended for. It also determines which AIF **Action** the message is executed against. The **SourceEndpointUser** must also explicitly set in the metadata for each message.

Now, we'll add in a send shape followed by a **Receive** shape to **Send** the **msgAXQueryExchangeRatesRequest** and receive the **msgAXQueryExchangeRatesResponse** messages. Now, connect the send and receive shapes to the **QueryAXExchangeRates_Port** port as shown in the falling image:

![](bbox=[0.28, 0.33, 0.72, 0.59])

Next, we'll add in an expression shape. Since we'll receive ALL the exchange rates corresponding to a **FromDate** that we specified in our query message, we need to determine whether the USD-based company is a corresponding entry for the CAD currency. The contents of the expression shape to do this are:

```
xpathQueryCADResultsExpression = System.String.Format("/*[local-name()='ExchangeRates']/*[local-name()='ExchRates']/*[local-name()='CurrencyCode' and text()='{0}']", "CAD");

// If we find a result, then don't need to load
if (xpath(msgAXQueryExchangeRatesResponse.ReturnValue,
xpathQueryCADResultsExpression) != null)
{
needToLoadFxRate = false;
}
```