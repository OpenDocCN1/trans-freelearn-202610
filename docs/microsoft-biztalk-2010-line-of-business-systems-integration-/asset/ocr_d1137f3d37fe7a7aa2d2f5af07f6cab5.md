The schema that will be used in our Typed Polling scenario includes an array of the result sets of the stored procedure. The schema is easy to configure if we want it further debatched by BizTalk Server. Our stored procedure was created to return 100 sales orders at the most with each poll. We can modify the schema created by the adapter so that the XmlDisassembler pipeline will split each of these repeating nodes into individual messages. The first step is to set the <Schema> node's Envelope property to Yes:

![](bbox=[0.285, 0.27, 0.715, 0.458])

Then, set the Body Xpath value of the TypedPolling node to the first TypedPollingResultSet0. This will tell the XmlDisassembler that anything below this record should be disassembled into a separate message:

![](bbox=[0.368, 0.54, 0.632, 0.76])

The previous two steps are all that need to be set during development for the TypedPolling schema to configure its messages to be debatched.