
Implemented correctly, the small upfront investment in writing tests will pay itself back tens or hundreds of times over. Even better, projects with tests are much less stressful to work on since you get notified of problems early when you have time to fix them without much fuss.

> **Health warning – calling a project agile doesn’t make it agile**
> There is a temptation for some managers to label a project Agile as an excuse to not do the hard work of project planning. But if you don’t invest in the tests, you will run into serious issues later (often when the original project managers have left). If you have had this experience, please keep an open mind for the next section – real test-first projects address a lot of the common pain points in developing technology solutions.

There is a lot of information available on Agile, Scrum, and test-driven development. We’ll focus in the next section on the tools the KIE project provides to support this approach. We’ll cover one tool built into the KIE sandbox and one more advanced approach that you might want to implement in Excel. Finally, we’ll preview some of the advanced testing features available in Business Central (which we cover in more detail in *Chapter 10*) and Visual Studio Code (which we introduce in *Appendix B* and *Chapter 12*).

## Run ... as Table

You may have noticed when you were running previous examples in the KIE sandbox using the **Run ... as Form** option, that there was a second option (**Run ... as Table**). Let’s look at that now, since it’s a good step into the testing approach:

1. We have the Bill of Materials option open in the KIE sandbox (downloaded model: `09 Bill of Materials.dmn`).
2. In the sandbox, select **Run ... as Table**. A screen like *Figure 9.13* will appear. Note that we’ve removed some columns for clarity and added some test values to show more clearly how the table runner works.

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_255_0.png)

Figure 9.13 – KIE sandbox table runner

3. Each line of this table is equivalent to using Run ... as Form as we tested our models with up until now. If we put the two values on the left (Qty and Product) as inputs, on the right, we can state the three outputs we expect (Grams of Cocoa in Product, Grams of Cocoa in PD: Please apply P-code style Order, and confirmation of the product name).

4. The convenience here is that we can see multiple different orders at once, which allows us to quickly check for anything going wrong.

While the table runner is a useful improvement, there are a couple of areas where it falls short of the test-first approach. The most important is that it does not flag where a test fails. For example, the third value in Figure 9.13 is a negative value, giving a spurious result. But it would be very easy to miss this – even if you spotted this the first time you looked at the image, could you guarantee to see the problem if this was one of the hundreds of output tables you needed to check?

Fortunately, KIE does give tools to fix this problem. We’ll cover those tools later in this chapter – but since they are not (yet) embedded into the KIE sandbox, it’s worthwhile discussing an Excel-based solution to this problem.

## Test cases using Excel

If you look at Figure 9.14, you’ll see we’ve evolved our table runner in Excel to solve the problem of rogue tests being missed. While it has the same values as Figure 9.13, it’s immediately obvious when there is a problem, as it gets highlighted in red.

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_256_0.png)

Figure 9.14 – A test scenario in Excel

This is a very simple idea, with four main groups of columns:

- Given are the values we are passing in
- Expected are the values we think should be returned by the decision service
- Actual are the values that came back in reality
- Flag is the column to check whether the Expected and Actual values matched

We won’t give a working example of running test cases in Excel since once you’ve grasped the key idea, the implementation is straightforward. You can set up this test spreadsheet, linking Excel with your decision service, using the approach you learned in Chapters 6 and 7.

The other details, such as conditional formatting, will be up to you, and the layout will be closely linked to the decision service that you have built.

It’s worth highlighting that testing in Excel is a completely feasible approach. The scenario simulation approach we discuss in the next section is better again – but it does depend on tools that you may need support in setting up. You might find the next section hard, but achievable. As you work your way through it, know that you always have a backup plan for testing in Excel.

## Scenario testing and simulation

Testing in the Excel approach is good, but it is even better to write tests using similar tools to the one we built our model with. That way, our tests sit near our decision models and much more likely to be used and updated. Figure 9.15 gives a sample of KIE’s Scenario Simulation tool, which takes this approach.

| # | Scenario description | GIVEN: Product | GIVEN: Qty | EXPECT: Bill of Materials | EXPECT: Product Details |
| --- | --- | --- | --- | --- | --- |
| 1 | Silk Tray | "Silk Tray" | 125 | 18750 | 150 |
| 2 | Crunch | "Crunch" | 10 | 100 | 10 |
| 3 | Crunch | "Crunch" | -10 | 0 | 0 |

Figure 9.15 – KIE Scenario Simulation editor

Figure 9.15 is very similar in approach to the Excel test scenario in Figure 9.14. It has the same inputs (GIVEN) and expected outputs (EXPECT). When we run the scenario simulation, it highlights any variances between expected and actual.

While this is a real screenshot from KIE, at the time of writing, the Scenario Simulation tool is not yet available in the sandbox. It is worth double checking, as the product roadmap is constantly evolving and may have changed by the time you read this. Instead, this Scenario Simulation tool is available in both of the advanced KIE decision modeling tools:

- **Business Central**, the older decision modeling tool from the KIE team, still has more features than the sandbox. As you’ll see in Chapter 10, Business Central is very accessible to business users. There are a couple of setup steps that most Excel Power users will be able to follow, but some will need support.

- **VS Code**, part of the toolkit aimed at developers rather than Business users. While many Excel users will be able to use it, not every reader of this book will want to take advantage of the power that it gives.

One key aim of this book is to give you a good foundation and clear next steps to explore further. VS Code from Microsoft is certainly one of these areas.

## What is VS Code and why use it to test?

You can use VS Code not just for writing decision models but also for a wide range of areas such as ML, web development, data science, and more traditional programming. To encourage you to take this next step, we'll introduce VS Code in the following section, and explain (in Appendix B) how to use it to get started writing decision models and scenario tests. That Appendix uses the online version of VS Code with no installation, to get you up and running more quickly.

VS Code is a free-to-use text editor and coding tool from Microsoft, with extensions that can handle pretty much any development project. In some of our earlier examples, we opened some files in a simple text editor such as Notepad. VS Code gives you a similar editor, but with the ability to have multiple text-like files open at once – look at the tabs across the top of the screen in Figure 9.16.

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_258_0.png)

Even better, VS Code gives you code coloring and syntax highlighting, which might appear to be simple decoration, but do make the file a lot easier to read. On the left of the screenshot, you'll also see that VS Code gives you a Windows Explorer-like view of all the files in your solution – including the decision model, scenario simulations (which can be seen in the main panel of the screenshot) as well as supporting files such as text, XML, and documentation in the .md markdown format.

The dark gray bar on the left of the screenshot hints at the power available in VS Code. The icons change depending on the plugins we have installed – in this case, we have added the DMN extension from the KIE team. Other icons on this bar allow you to interact with GitHub repositories (in a more powerful way than we can do in the KIE sandbox), deploy items to Azure, and run individual files in your project.
