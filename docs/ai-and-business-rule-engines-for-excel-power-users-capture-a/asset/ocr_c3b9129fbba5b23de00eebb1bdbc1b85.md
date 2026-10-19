Here, we have input a quantity of Silk Tray – this is quite a heavy product, with a lot of chocolate in it so it’s not surprising that it will take 18.75 kg of cocoa to manufacture it in total. The KIE sandbox displays the context in a clear key:value format – compare this to our list in Figure 9.1, which has a similar format but has numbers instead of keys beside each of the values.

# More reading on FEEL functions

This may be a surprise for a book that focuses on KIE from Red Hat and IBM – but I would also suggest looking at the FEEL guides from other vendors such as Oracle, Trisotech, and Camunda.

Since FEEL is a standard, most of what they say is applicable to KIE – and each explains the concepts from a slightly different angle. It’s a good way of helping you if you get stuck in any area.

This section focused on highlighting the difference between FEEL and Excel functions, walking through some areas that Excel users might find tricky to understand. The best way of learning is to dive in and try to model some decisions that apply to your organization.

Before we leave functions, we’ll look at another expression language – Power FX.

# Using Power FX expressions in decision models

**Power FX** is a low-code expression language from Microsoft that is not too far from what FEEL tries to achieve. Roughly speaking, if you could take the formula you were already using in Excel, and remove the references to cells so that they were no longer tied to a spreadsheet, you would end up with something close to the Power FX language. This means that you can use Power FX in Power Apps to build mobile applications and, increasingly, the syntax will feature in Power Automate and other Microsoft platforms. More details on the language are at https://learn.microsoft.com/en-us/power-platform/power-fx/overview.

Power FX goes further than FEEL – whereas FEEL is written to have no side effects, Power FX is quite happy to have an impact (for example, to update a cell value or to change the color of a range). Power FX also integrates with a wider range of systems than FEEL, which is focused on decision models.

Using Power FX expressions is currently an experimental feature in KIE – as described by the team in the following post: https://blog.kie.org/2022/03/using-javascript-and-power-fx-with-dmn.html.

So far in this chapter, we’ve focused on expression languages that we can use in our models. Let’s take a wider look at some more advanced features for designing and building our decision models.

# More on building and editing decision models

In previous chapters, we looked at building decision models and extended this knowledge by exploring the FEEL expression language. This section is going to extend our knowledge of decision models by looking at best practices.