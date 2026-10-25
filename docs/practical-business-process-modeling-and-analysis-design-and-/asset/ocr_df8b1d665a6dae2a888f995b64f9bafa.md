# # 330 Advanced BPMN

While the graphical model showing the connections between decisions, input data, and other elements can be useful for a better understanding, it does not help us solve our problems with the complexity of decisions. For this purpose, we need another DMN element: **decision tables**.

A decision table allows us to show, clearly and unambiguously, which combinations of input data lead to which outputs (i.e., the results of our decision). This is very useful as documentation, but even more powerful when used for automation.

![](bbox=[0.133, 0.233, 0.867, 0.411])

Figure 9.40 – Decision table

_Figure 9.40 shows a simple example of a decision table. Each row represents a rule. Columns 2 (**Purchase confirmed**) to 6 (**Fraud possible**) represent inputs needed to make a decision. Column 7 (**Routing**) shows the output, which is the result of our decision. Column 8 (**Comments**) allows us to add some background information to those rules._

As you can see, decision tables are meant to be easy to read and use. For example, the first row shows that if there is no confirmation that the item was bought from our company, the complaint will be automatically rejected, and we do not care about other inputs.

The second row is also easy to read. If we know that the customer bought from us and that the item could be faulty due to our error (either in production, packaging, or shipping), we want to save time for the customer and the company, and we accept the case of known issues automatically.

In rows 3 to 5, things start to get more interesting. Purchase is confirmed, but this time, we are not sure why the problem occurred. That’s why we will need to ask our employee to investigate the case.

Those three rows are needed because we achieve the same results, but based on different inputs. Row 3 shows that if the value of the complaint is over 50 USD, we want a manual check due to the high value. Rows 4 and 5 cover cases when the value is up to 50 USD, but we have different categories of customers, while for long-time customers, we are concerned about possible fraud.

To understand those rules better, we need to learn about an additional important concept covered by DMN: **Friendly Enough Expression Language (FEEL)**. Let’s take a look at the values in the **Complaint value** column. In row 3, we have > 50, while in rows 4 and 5, we have <= 50. As you may expect, this shows for which complaint amounts which rule will be applied.