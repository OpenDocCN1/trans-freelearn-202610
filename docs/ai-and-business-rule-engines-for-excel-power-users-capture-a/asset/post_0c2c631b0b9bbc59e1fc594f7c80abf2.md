
Before you rush off to expand this example, we want to leave you with some key steps for troubleshooting. Sooner or later, your M formulas will break, and these steps will help you find and fix the problem:

- We’ve seen already how to break down our query step by step, even if our main query does everything end to end. That lets us check whatever we like if we read our parameters in correctly.
- If we need to see interim values (for example, the JSON Payload that we are generating), we can change the last line of the query to return Payload instead – just remember to change it back.
- We can use comments such as // or /* multiple lines*/ to comment out code we want to skip.
- In this example, we display the values as a table, but in reality, we would manipulate them further in Excel – using the formula =TableName[column_name], which we saw before to extract key values.

If you follow these steps, any problem you may encounter will become much easier to solve. A bigger problem is knowing when you should use Power Query instead of the other approaches we have mentioned.

## When to use Power Query

There is a reason why we started with Power Query as the first option for linking Excel with our decision service. It’s the closest approach to an Excel formula, and the M query language is very powerful for building out your solution. Even better, all the tools needed are built into Excel – so any user who receives your spreadsheet will be able to use it.

However, the fact that everything is packaged in Excel is also a reason to not use it. Users have full control over your spreadsheet (even if you attempt to lock it down). Most of the time, these changes are well intentioned but clumsy, but there have been instances of fraud by people manipulating Excel spreadsheets to hide what they were doing.

You’ll see in the next chapter that we will introduce other scripting-based approaches. Power Query is still likely to be an approach you’ll consider, but there may be better options depending on the solution that you’re trying to build.

## Summary

This chapter bridged the Excel world that you were already familiar with and the world of AI, business rules, and decision services that you learned about in previous chapters. To do this, we did the following:

- Introduced five different ways to link rules, AI, decision models, and Excel.
- We learned about machine-readable web pages using REST and how to link to our decision service using Swagger – a more human-friendly link to Kogito decision services.
- We called our product recommendation decision service using Power Query. The step-by-step examples we gave are a solution that you can expand on for your own work.

Even if you stopped reading this book now, you would already know how to untangle Excel spreadsheets and move business rules into an external decision service.

So what is left to cover in the second half of this book? We’ll cover techniques to do this in a better, faster, more flexible, and more scalable way. We’ll start with an area we mentioned in this chapter – linking Excel to our decision service using more powerful scripting-based approaches (Visual Basic, Script Lab, or Office Scripts).

## Part 3:
Extending Excel, Decision Models, and Business Process Automation into a Complete Enterprise Solution

Even if you stopped reading the book after the previous section, you would have an end-to-end solution to include business rules and AI in Excel. But the tools have so much more power, well worth exploring further to build scalable, robust, and enterprise-level solutions.

This section includes the following chapters:

- *Chapter 7, Using Business Rules in Excel with Visual Basic, Script Lab, and Office Scripts,* offers three alternative script-based methods to give you more choices about how you link Excel with your business rules.
- *Chapter 8, AI and Decision Services within Excel and Power Automate Workflows,* introduces Microsoft's business automation tool and shows how we can use our models to make decisions at key points in a flow.
- *Chapter 9, Advanced Expressions, Decision Models, and Testing,* explores the full power of the expression language built into KIE and introduces the scenario testing tools so we can guarantee our decision models behave as we expect.

## Using Business Rules in Excel with Visual Basic, Script Lab, or Office Scripts

In the previous chapter, we said that no matter what technique we use to link Excel to our decision service, the approach is similar. We'll prove that in this chapter. Power Query is a great approach but has limitations. We'll explore linking Excel and decision services using Visual Basic, Script Lab, or Office Scripts – which approach you use is your choice. Along the way, we'll cover the following topics:

- Calling decision services from Excel using Visual Basic for Applications
- Installing Microsoft Script Lab and using it to link Excel with our decision service
- Introduction to Office Scripts and making the link to our decision service using that Excel script online

Why are we covering multiple ways of doing essentially the same thing? We'll explain that in this chapter as well, exploring the strengths and weaknesses of each approach.

Since it has by far the largest install base of all the approaches, a good place to start this chapter is with Visual Basic for Applications (VBA).

## Calling decision services using Visual Basic for Applications

There are plenty of good reasons to consider linking Excel and your decision service using VBA. You may have many years of experience in Visual Basic. Or you may have inherited a code base that uses a lot of VBA logic, and your organization isn't ready to migrate away from it. Since this chapter assumes familiarity with Visual Basic, if it's been a while since you used it, take a look at Appendix A, for a refresher.

## Do I really need to know about Visual Basic for Applications?

The simple answer to that question is no. If this is your first time scripting in Excel, you don’t need to know VBA since it is a legacy solution. Unless you or your company already use Visual Basic extensively, please skip to the next section, *Meet Microsoft Script Lab – a modern version of VBA*, to read about a better alternative.

I have a lot of fondness for Visual Basic. It formed an important stepping-stone in my own career and still powers many Excel spreadsheets worldwide. So, it is with the greatest respect that I recommend you *avoid* using VBA wherever possible. Even in this short section, you’ll recognize its limitations – security restrictions that you cannot control on users’ machines, the ability to run only on desktops/laptops (not in Office 365), and its clumsiness in sending and receiving JSON data over the web. Remember that Visual Basic was born before the internet took off, and its age shows that.

On a positive note, migrating business logic out of VBA and Excel and into Kogito decision services is a good first step to making the situation better. So, if you have one of these many older spreadsheets, or feel this is a route you must take, please read on! We need to start with something that the users of your spreadsheets will encounter – security checks.

## Security checks in running Excel macros

I feel compelled to give you a stern warning before you download the first sample for this chapter from the usual GitHub site: https://github.com/PacktPublishing/AI-and-Business-Rules-for-Excel-Power-Users.

Immediately after downloading the sample file, 07_Decision_Service_VBA.xlsm, Windows will give a security warning. This is because Excel files containing VBA from malicious authors have been known to act as a virus.

Compounding the problem is that Excel files are binary files, and the macro can be set to auto-run when you open them. You will not get the chance to inspect them properly like the other book examples, which you can open in a normal text editor before running. Remember the earlier caution we gave about trying to avoid using VBA if possible? The other approaches later in this chapter do not have these security limitations.

## Security warnings can halt the adoption of your solution

Keep in mind that any user you distribute your Excel workbook to (including the VBA scripts) will get similar security warnings. From personal experience, these warnings cause friction with users and will interfere with the successful rollout of a VBA-based solution – no matter how good it is.

While I’d like to think you trust me as an author (after all, we’ve got to this point in the book together), encouraging you to execute VBA files you’ve downloaded from the internet is a bad idea. So, you might want to skip the download and just read through the text descriptions on the following pages instead.
