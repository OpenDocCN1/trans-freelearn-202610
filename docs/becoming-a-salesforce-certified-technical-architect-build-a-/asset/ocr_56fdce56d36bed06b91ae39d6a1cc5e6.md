# Learning about Data Architecture Considerations and Impact, and Building your Solution and Presentation

Give yourself time to quickly skim through the scenario, understand the big picture, and develop some initial thoughts about the solution. Once you have done this, you are ready to go through it again, section by section, and incrementally build the solution.

# Understanding the Current Situation

The first paragraph contains some general information about POW’s business model. It also contains some interesting figures, such as 5 million daily visitors and 150,000 orders per day, with an average of five items in each order. Take some notes about these figures. In some scenarios, there are figures that are not necessarily going to impact the solution from a technical perspective. However, they could be mixed up with other figures, which you should be careful writing about.

For example, at first glance, you will notice the huge number of daily visitors, which could impact the governor limits in many ways. It is still unclear which governor limit is at risk at this stage but make note of this number and move on. After all, it might simply be a figure impacting an off-platform, third-party solution.

The number of daily orders is also massive. Keep that on your radar too. Start scribbling your data model diagram. You do not know yet whether any of these objects would end up in Salesforce, but nevertheless, having that diagram will help you make the right decisions once you go further into the scenario. What you know so far is that you are likely going to use the following objects: `account`, `contact`, `product`, `order`, and `order line item`.

The first and second paragraphs are also helpful in building an early understanding of the system landscape. So far, you know that there is a website being used to list, display, and capture orders. There must be an `order fulfillment system`, a `shipment system`, and an `invoicing system`. You do not know exactly which of these functionalities will be fulfilled by Salesforce and which will not, but capture these requirements and add the assumed systems to your initial landscape architecture diagram.

Once an order has been placed, it can be fulfilled either via POW or the traders themselves, depending on the preferences provided by the traders for each of their products.

You can see a clear description of one of the business processes. The business process diagram is not as essential as the other diagrams for the review board, Still it is handy to help you tie back your solution to a business process, which, in turn, will help you present the solution in an engaging manner. The business process diagram is a must-have in real-life projects as it can significantly improve the quality of the client requirements you have gathered. Start creating your draft business process diagram. The next paragraph completes this picture:

POW stores the bank account details of all its traders. Once the item is shipped to the consumer (either by POW or the traders), POW automatically calculates its commission, deducts it from the collected payment amount, and deposits the remainder into the trader’s bank account.