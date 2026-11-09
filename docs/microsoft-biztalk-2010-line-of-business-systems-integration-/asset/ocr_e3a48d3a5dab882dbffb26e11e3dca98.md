If we had a synchronous scenario with our client application, then we would not have much time to retry calling SAP before the client times out. In these synchronous situations, I recommend setting the SAP's Send port retry value to 0 and then ensure I return a response back to the client indicating that SAP was not available. This provides a few benefits:

+   - We are not trying to retry connecting to SAP when the client has already timed out and severed its connection to BizTalk.
+   - If SAP is down, BizTalk will be able to immediately detect this situation and send a negative acknowledgement message back to the client application. Otherwise, we would not know this until BizTalk had exhausted its retry threshold.
+   - Client system can then provide a clear, user friendly error message to the client indicating that there is a problem.

# Building a BizTalk application—BAPI walkthrough

We are now going to expand on our RFC scenario and update our existing Chapter05-RFCBAPI project. Once our customer receives a quote from our quoting system and has decided to proceed with our company doing the work, we want to leverage SAP's Sales and Distribution capabilities. SAP also has strengths in Sales and Financials so instead of trying to build that functionality in our Quoting system we want to take advantage of SAP's core capabilities in this area.

The Quoting system will create a Sales Order request, and provide information about the customer, and the materials associated with the job. In return, SAP will create the sales order that can be used for billing purposes and return an order number back to the Quoting system.