# # Names, LET, and LAMBDA

If you’ve ever thought, “Oh, I wish Excel had a way to automate calculating x,” your day has arrived. There will be some nuances and increasing complexity along the way, which is why I’ve chosen a straightforward path for us to follow. We’ll start with simple multiplication to calculate the volume of a box, and then riff on that calculation several ways as we make our way to the LET and LAMBDA functions. I’ll introduce the concept of Names, which are sometimes referred to as range Names. Names can streamline formula writing and make it easier to determine what a formula references and is calculating. The LET and LAMBDA functions share some characteristics with Names. For the benefit of anyone using an older version of Excel, I’ll also briefly discuss creating custom worksheet functions with Visual Basic for Applications programming code.

In this chapter, I will cover the following topics:

+   - PRODUCT, IF, CHOOSE, SWITCH, IFERROR, and ISERROR functions
+   - Assigning Names worksheet and inputs
+   - Introducing the LET function
+   - Introducing the LAMBDA function
+   - Going deeper with LAMBDA functions
+   - Custom VBA worksheet functions

By the end of this chapter, you’ll be empowered to create custom worksheet functions in Excel. Along the way, you’ll be able to compare the pros and cons of each approach, and hopefully expand your Excel toolbox as well.

# ## Technical requirements

The IF, CHOOSE, and PRODUCT functions, as well as Names, work in any version of Excel. The SWITCH function requires Excel 2019 or later, while the LET function requires Excel 2021 or Microsoft 365. The LAMBDA function works in Microsoft 365 and Excel for the web. There are two example workbooks for this chapter: the Chapter 11 – Names, LET, and LAMBDA.xlsx workbook