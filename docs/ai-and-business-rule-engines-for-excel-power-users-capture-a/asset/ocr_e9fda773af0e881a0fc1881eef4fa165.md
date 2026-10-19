+   3. Each line of this table is equivalent to using Run ... as Form as we tested our models with up until now. If we put the two values on the left (Qty and Product) as inputs, on the right, we can state the three outputs we expect (Grams of Cocoa in Product, Grams of Cocoa in PD: Please apply P-code style Order, and confirmation of the product name).

+   4. The convenience here is that we can see multiple different orders at once, which allows us to quickly check for anything going wrong.

While the table runner is a useful improvement, there are a couple of areas where it falls short of the test-first approach. The most important is that it does not flag where a test fails. For example, the third value in Figure 9.13 is a negative value, giving a spurious result. But it would be very easy to miss this – even if you spotted this the first time you looked at the image, could you guarantee to see the problem if this was one of the hundreds of output tables you needed to check?

Fortunately, KIE does give tools to fix this problem. We’ll cover those tools later in this chapter – but since they are not (yet) embedded into the KIE sandbox, it’s worthwhile discussing an Excel-based solution to this problem.

# Test cases using Excel

If you look at Figure 9.14, you’ll see we’ve evolved our table runner in Excel to solve the problem of rogue tests being missed. While it has the same values as Figure 9.13, it’s immediately obvious when there is a problem, as it gets highlighted in red.

![](bbox=[0.13, 0.61, 0.88, 0.68])

Figure 9.14 – A test scenario in Excel

This is a very simple idea, with four main groups of columns:

+   - Given are the values we are passing in
+   - Expected are the values we think should be returned by the decision service
+   - Actual are the values that came back in reality
+   - Flag is the column to check whether the Expected and Actual values matched

We won’t give a working example of running test cases in Excel since once you’ve grasped the key idea, the implementation is straightforward. You can set up this test spreadsheet, linking Excel with your decision service, using the approach you learned in Chapters 6 and 7.