
# Figure 4.3 – Data Type editor in KIE

4. Click on **Add a custom Data Type** – the blue box near the bottom of the screen.

5. Enter the name **tCustomer**. While we can choose almost any name we like, the **t** at the start (which stands for type) will make it easier to distinguish this as a data structure later on.

> **What is a type?**
> Think of a type as a cookie cutter. With one cutter, we can cut out and bake as many cookies as we want.

6. In the drop-down menu, we choose a data type of **Structure**. The screen should look similar to Figure 4.4:

# Custom Data Types

New Data Type

tCustomer Structure

# Figure 4.4 – New Data Type

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_101_0.png)

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_101_1.png)

7. Our **tCustomer** is equivalent to one line in the earlier Excel spreadsheet. We still need to tell KIE about the individual pieces of information within each customer type. Click on the pencil icon to add a customer number of a **number** type, then click on the tick icon to save.

8. Click on the + icon (underneath the customer number) to add the following types in turn, as shown in *Figure 4.5*:

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_102_0.png)

Figure 4.5 – Custom Data Types

Congratulations -- you've just created your first **custom data type** in the KIE sandbox. Now that we’ve created a **custom data type**, let’s see what it looks like in a Decision Model.

> **Is customer name really needed on our data type?**
> 
> In real life, we’re unlikely to pass in personally identifiable information such as customer names. This is because our decision service doesn’t store any information -- it just makes a judgment on the information you pass to it.
> 
> You shouldn’t (for many reasons) make a decision based on a single customer name. A more likely scenario is that for very important customers, you might create a **Special customer** Boolean field.
> 
> These design choices are especially important when we begin to consider the ethical use of customer data, which we’ll touch on in the next chapter.

## Using our complex data type in a decision model

To start using our tCustomer data type in a decision model, do the following steps:

- 1. Start by returning to the Model Editor screen by clicking on that tab.
- 2. In the editor, drag an Input data node and call it Customer – remember our cookie customer example? tCustomer is the cutter, we’re going to use it to create an actual customer shape.
- 3. Click on the properties icon (remember – it’s on the top right of the editor). In the Properties pane, specify that we want to use the tCustomer data type we just created. It should look similar to Figure 4.6:

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_103_0.png)

Figure 4.6 - Using our data type in a Decision Model

We mentioned before that structured data types like tCustomer are part of a contract with people calling this decision service. To demonstrate this, if we click on the Run .. as form button, we can see that KIE has automatically generated a data input form based on our structured data type, as shown in Figure 4.7:

2. The **Data type constraints** screen appears. In *Figure 4.9*, we’ve already selected **enumeration**, which allows us to specify a list of values. And then we entered the text that is acceptable for use in our **Special Request** field: **diabetic**, **vegan**, or **lactose intolerant**. Note that it is also possible to enter the list of values as an **expression** type (if you choose this route, the Sandbox will give you an example to help guide you).

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_105_0.png)

Figure 4.9 – Complex data type with constraints

3. Remember to click **OK**, or the green arrow in the constraint editor to save our updates.

When we go back to the **Run** menu, you’ll see (*Figure 4.10*) that KIE has recognized the new contract and updated the user interface so that you can only select values that align with this restraint:

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_105_1.png)

Figure 4.10 – Updated data input form suggesting constrained values

You’ll notice that our sample (as modeled in KIE) does not yet match the Customer (with Orders) we modeled in JSON and Excel. Since you’ve already used the data type editor, creating an order object and nesting it within the customer is easy:

- 1. Create the **tOrder** type, with the fields you need.
- 2. Edit the **tCustomer** data structure and add a (Previous) **orders** field using this **torder** type.
- 3. Edit the **list** slider, which allows us to add multiple previous orders per customer.

As before, the auto-generate form on the **run** screen will update – try it out! The downloadable copy of the data type we created is available as 04_Customer_Data.dmn on the book’s GitHub page https://github.com/PacktPublishing/AI-and-Business-Rules-for-Excel-Power-Users.

Downloading it will make it easier if you don’t want to work your way through all these steps – but remind yourself of the GitHub download instructions we gave in the previous chapter to avoid the most common mistakes.

Just because you can create extremely complicated custom data using the editor doesn’t mean that you should. Let’s think about what makes for a good design.

## What makes a good custom data type?

Since you’re building a service that other people (including you in the future) will use, it pays to make it as easy to use as possible. There are entire books written on good data design, but at this stage, a couple of simple guidelines will go a long way:

- - It’s better to follow how people talk in day-to-day business – customers, delivery addresses, orders, product recommendations, and other similar terms. Familiar, clear names makes it easier for people to understand since you mirror the way they already think.
- - Several simple data types are better than one large complex one. Not only are they easier to understand, but it makes it easier to reuse your decision service.
- - Data modeling can get very complicated very quickly. Copying existing spreadsheets and systems is a good start. Consistency is better than the confusion of having a slightly different data model.
- - Keep the names simple – our type used **number** instead of **customer_number** since it was already clear from the context that it was associated with a customer.

Above all – don’t be afraid to experiment. At this stage in the book, we’re prototyping – playing with the technology with a focus on learning. And at some point, we will probably (re)start with a *clean-sheet* design. This clean-sheet design will be built a lot faster as we will have a much better idea of what we are doing at that point.

Now that we can use data types to specify the data we expect to be passed in, let’s use it in a more powerful Decision Model using Decision Tables.

## A more powerful Decision node – Decision Tables

In Chapter 1, Wrestling with Excel? You Are Not Alone, we introduced business rules as decisions we can make using a when ... then format, for example; When time is AM - say Good morning.

We are likely to have more than one business rule - otherwise, why bother using a rule engine? Decision tables are just a format to write multiple, similar business rules in a table format, like this:

| First | Input Value 1 | Input Value 2 | Output |
| --- | --- | --- | --- |
| 1 | When value1 = "Something" | AND value2 > 100 | "Matched" |
| OR 2 | When value1 = "Completely different" | AND value 2<100 | "Different match" |
| OR 3 | - | Value 2 = 100 | "Match on number only" |

Table 4.1 – Business rules mapped as a simple Decision Table

Where are the Input Values coming from? That’s the data model we constructed earlier. The arrows we use to wire different nodes together mean that KIE knows what data is available for use by the decision table (and will try to autogenerate the table for you).

Drools (the rule engine underlying KIE) takes these input values and tries to find the line in the Decision Table with the best match. There are different methods (HIT Policies) for finding the best match. We’ll use First in our example since it is simple to understand, but we’ll come to policies for matching later.

When it finds a match, the value in the Output column is returned. We can have more than one output value, but we’re keeping things simple here.

Note that not all values need to be provided. The hyphen, -, in line three of Table 4.1 tells KIE not to match in this column for this rule. There are a range of friendly enough expression language (FEEL) expressions we can use in the values we want to match, which means that any text we want to match has to be surrounded by quotes.

We don’t actually use AND or OR in our decision table syntax – they are implied by the layout of the table. We include them here for clarity. Likewise, we just write something as the pattern we want to match (see Table 4.1 - the first column of our first rule).

Now that we’ve understood the idea of a decision table, let’s capture our first decision table using the tools in the KIE Sandbox.

## Our first Decision table in the KIE Sandbox

The graphic editor in the KIE Sandbox makes creating our decision table easy. We’ll start using the data model we created in our previous exercise earlier in this chapter. This is also available on the samples page as 04_Customer Data.dmn.
