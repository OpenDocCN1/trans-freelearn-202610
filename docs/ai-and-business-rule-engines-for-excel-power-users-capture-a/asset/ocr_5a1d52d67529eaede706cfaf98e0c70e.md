These options include creating a blank new snippet and reviewing projects you previously created in **My Snippets**. There are also many well-documented examples under **Samples** that are well worth working through to explore Script Lab further. Finally, we'll use the **Import** option in the next section to quickly bring in the examples that we'll use later in this chapter.

As we finish our tour of Script Lab, it's worth taking a quick look at the Microsoft documentation to learn more. A good starting point is https://docs.microsoft.com/en-us/office/dev/add-ins/overview/explore-with-script-lab.

# Saving projects in Script Lab

Let's repeat something we mentioned in the previous section: Script Lab projects are not saved in the location that you may expect:

+   - Unlike in VBA projects, Script Lab projects are not saved as part of the current Excel sheet. This is a good thing as other users cannot modify or break the script. But it does mean that users need to install Script Lab to import the scripts. This is not a big deal but involves a separate step that we'll describe shortly.
+   - Scripts are not (as you also might expect) saved in the Excel folder on your laptop. This is because they need to work with the Office 365 version of Excel as well. In effect, they run within a web browser that is itself embedded in Excel, so both the online and desktop versions have a similar environment to work within.
+   - Because of this, Script Lab projects are hosted and saved within the web browser's local storage, not unlike KIE Sandbox. Like KIE Sandbox, there is the option to save the scripts automatically to GitHub using gists. We won't cover saving it in detail since it is so straightforward, using the **Login** and **Share** buttons we encountered previously.

Having the script separate from the Excel spreadsheet gives you more options. It is possible for you to give instructions on how to install Script Lab to all users.

But more likely, you would share the spreadsheet as normal, and then a smaller number of power users would perform the checks using the scripts. For example, a mortgage broker might pass out a loan application form in Excel to all clients, and only run the script on the returned spreadsheets. That allows the broker to keep their business knowledge about which loan to recommend private.

# Importing scripts into Script Lab

We previously shared and saved a Script Lab snippet in the book's sample GitHub folder as `07_Script_Lab_Hello_world.yaml`. We're going to use that in the next section, so it's worth importing that script into Script Lab now, as follows:

+   1. Download and open this script in an editor such as Notepad.