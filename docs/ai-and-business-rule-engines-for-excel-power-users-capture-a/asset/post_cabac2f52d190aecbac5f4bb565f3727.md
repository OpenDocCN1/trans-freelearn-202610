
- **Relations**: These are similar to lists but can group multiple values together, as shown in Figure 4.17. Each row in this relation contains useful information about our product. And each column is effectively a list, which is useful if we want to access a list of all our products:

## Product Info (Relation)

| # | Product (string) | Weight in Grams (number) | Vegan Suitable (boolean) |
| --- | --- | --- | --- |
| 1 | "Peanut Crunch" | 75 | true |
| 2 | "Caramel Biscuit" | 125 | false |

Figure 4.17 – Relation Decision Node showing product information

## Relations are not Decision Tables

Because of the table format, relations look very similar to Decision Tables. Despite their appearance, they work very differently. Relations return all of the information they contain. Decision Tables match the best line(s) and return only that information.

Relations are great for defining information once and reusing it across many decision models. In practice, this table of product information would be pulled from an external system source using an invocation. For now, modeling the information as a relation in one place works well. It also makes it easier to change later as our system evolves.

## Play around with the nodes

Most of the nodes we presented in this section are quite easy to understand once you start using them. We encourage you to play around with them in the KIE Decision editor and see the outputs using the Runner. For example, Figure 4.18 shows the output of our Product Info relation node when we run it:

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_115_0.png)

Figure 4.18 – Output from a Relation Decision Node showing product information

We’ll use some of these expressions and nodes in our examples to help you understand them better. Since all the nodes are part of the **Object Model Group (OMG)** Decision Model Standard, there is a lot of information and examples about each of them online. A good place to start is the learn DMN site (which is part of KIE): https://learn-dmn-in-15-minutes.com/learn/decision-logic. There is more detailed information on the Drools site (also part of KIE) at https://www.drools.org/learn/dmn.html.

## Adding nodes to support our Decision Table

Our previous decision table example was simple: one data input node and a table generating an output. Since the power of Decision models comes from combining many nodes together, let’s start building an example that links many nodes.

This example will allow us to recommend different chocolate bars depending on which day of the week the customer was born on. This comes from a real-life example and is probably no crazier than some of the business rules that you have been, or will be, asked to model.

We'll start our work based on our previous decision table example, downloadable as `04_First_Decision_Table.dmn` from this book's GitHub site:

- 1. Open the Decision Model editor, and add the **DayCalc** Decision node.
- 2. Create an arrow to connect existing **Customer Data Note** to feed data into this new **DayCalc** node.
- 3. Create another arrow to connect the **DayCalc** node to the existing **Recommended Product** table.
- 4. Open the **Properties editor** on the **DayCalc** node and set the output to **string**. It should look like *Figure 4.19*:

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_116_0.png)

Figure 4.19 – Creating a Function Decision Node

- 5. Next, edit the **DayCalc** node by clicking on the pen icon next to the node.
- 6. On the screen that appears, set **Expression type** to **Function**.
- 7. Edit the parameters of this function by clicking on the **Edit Parameters** box. In the pop-up menu, click on **Add parameter**, as shown in *Figure 4.20*:

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_116_1.png)

Figure 4.20 – Adding parameters to the function

- 8. Enter a parameter named DOB and give it a data type of **Date**. Click anywhere to exit the whitespace menu.

9. Now that you are back at the node editor, enter day of week (DOB) in the expression box. You've guessed it – this is a FEEL expression that returns the day of the week (for example, Monday or Tuesday) linked to this date. With this edit, your node should look similar to Figure 4.21:

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_117_0.png)

Figure 4.21 – Entering the function

If we have entered our formula correctly (no spelling mistakes), you should see an output in the Run panel of function DayCalc( DOB ) - Evaluated with success.

While it's useful to know that KIE can understand our function, we haven't yet called it by passing in a date and received a response.

## Running our function

While we've wired our decision model to make the function available to the decision table, it is helpful to see exactly what is going on. To do this, we're going to add another node to our decision model to allow us to visualize the output:

- 1. Open our previous example, add another decision node, and call it Customer Birth Day.
- 2. Link the new node from the DayCalc function. Your model should be similar to Figure 4.22:

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_117_1.png)

Figure 4.22 – Creating a Decision Node to test the function

- 3. Open the properties of **Customer Birth Day** to give it a data type of **string**.
- 4. Edit the **Customer Birthday** decision node, and set **Logic type** to **Context**. Of course, there are many different ways of calling a function, but this is a useful excuse to play with context decision nodes.
- 5. In the screen that appears once you’ve chosen the context type, add entries as follows, so it looks like *Figure 4.23*. You will need to insert new lines and select the data type of **string** from the drop-down menus. `DayCalc(Customer.Date of Birth)` and `Day of Week` are literal expressions:

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_118_0.png)

- 6. Open the **Run** panel and select a date from the date picker on the **Date of Birth** input field – for example, **2000-01-01**.
- 7. Immediately, KIE will evaluate the decision model, including the **Customer Birth Day** node we just created. We’ll see what day of the week the customer was born on; in this case, it was a Saturday.

> **Tricks of the trade**
> 
> Adding an output node so we can see what is going on is a useful tool for debugging (trying to find why something is not working the way we want it to). Since nobody gets things right the first time, we talk more about helpful testing techniques in *Chapter 9*.

The **context** node we used was pretty simple: one calculation and an output line. If needed, we could have added additional lines and have KIE build up the result step by step. Again, it’s worth playing with this example in the KIE editor before we move on to build a more sophisticated Decision Table.

## A more sophisticated Decision Table

We added a function node to calculate the day of the week our customer was born on. But we still haven't used that day of the week to recommend a suitable product. Let's update our previous example (downloadable as 04_Customer_Birth_Day.dmn) to make this happen:

- 1. Open the Decision Model, right-click on the **Recommended product** decision node, and choose the edit icon.
- 2. Add **Input Clauses** (pale blue) so that you have the three input columns, as shown in Figure 4.24. You can do this by right-clicking on an existing input column and choosing **Add right**.
- 3. Double-click on each input clause to set where the data is coming from, as shown in Figure 4.24. `Customer.Country of Residence` and `Customer.Number` come directly from our data model. `DayCalc(Customer.Date of Birth)` calls the function (once) that we created and makes the result available to match against our rules.
- 4. We've resized some columns and dragged and dropped them to re-order them – but this doesn't change the rule's functionality.
- 5. In the main body of the table, enter the values as shown. Pay attention to the quote marks needed for text values.
- 6. The - values denote where no match is attempted, and the annotations are just comments to explain what is going on but are otherwise ignored.

RecommendedProduct (Decision Table)

| F | Customer.Country of Residence (string) | DayCalc(Customer.Date of Birth) (string) | Customer.Number (number) | RecommendedProduct (string) | annotation-1 |
|---|----------------------------------------|------------------------------------------|--------------------------|-----------------------------|--------------|
| 1 | - | - | <10000 | "Silk Tray" | Only first 10000 custom |
| 2 | "SA" | - | - | "Lumpy Fruit and Nut Bar" | |
| 3 | "UK" | "Friday" | - | "Crunch Bar" | |
| 4 | "US", "UK" | - | - | "Peanut Candy" | |
| 5 | - | - | - | "Milk Chocolate" | |

Figure 4.24 – A more sophisticated decision table

7. Open the **Run** panel to evaluate our model. As expected, **US** customers get recommended **Peanut Candy**. **UK** customers get recommended a **Crunch Bar** – but only if they were born on **Friday**. Otherwise, customers will receive the default suggestion of a **Milk Chocolate** bar. And any person who is in the first 10,000 customers gets recommended our special **Silk Tray** offer.
