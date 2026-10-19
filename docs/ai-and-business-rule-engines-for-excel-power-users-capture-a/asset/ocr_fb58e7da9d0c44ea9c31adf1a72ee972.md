Before you rush off to expand this example, we want to leave you with some key steps for troubleshooting. Sooner or later, your M formulas will break, and these steps will help you find and fix the problem:

+   - We’ve seen already how to break down our query step by step, even if our main query does everything end to end. That lets us check whatever we like if we read our parameters in correctly.
+   - If we need to see interim values (for example, the JSON Payload that we are generating), we can change the last line of the query to return Payload instead – just remember to change it back.
+   - We can use comments such as // or /* multiple lines*/ to comment out code we want to skip.
+   - In this example, we display the values as a table, but in reality, we would manipulate them further in Excel – using the formula =TableName[column_name], which we saw before to extract key values.

If you follow these steps, any problem you may encounter will become much easier to solve. A bigger problem is knowing when you should use Power Query instead of the other approaches we have mentioned.

# When to use Power Query

There is a reason why we started with Power Query as the first option for linking Excel with our decision service. It’s the closest approach to an Excel formula, and the M query language is very powerful for building out your solution. Even better, all the tools needed are built into Excel – so any user who receives your spreadsheet will be able to use it.

However, the fact that everything is packaged in Excel is also a reason to not use it. Users have full control over your spreadsheet (even if you attempt to lock it down). Most of the time, these changes are well intentioned but clumsy, but there have been instances of fraud by people manipulating Excel spreadsheets to hide what they were doing.

You’ll see in the next chapter that we will introduce other scripting-based approaches. Power Query is still likely to be an approach you’ll consider, but there may be better options depending on the solution that you’re trying to build.

# Summary

This chapter bridged the Excel world that you were already familiar with and the world of AI, business rules, and decision services that you learned about in previous chapters. To do this, we did the following:

+   - Introduced five different ways to link rules, AI, decision models, and Excel.
+   - We learned about machine-readable web pages using REST and how to link to our decision service using Swagger – a more human-friendly link to Kogito decision services.