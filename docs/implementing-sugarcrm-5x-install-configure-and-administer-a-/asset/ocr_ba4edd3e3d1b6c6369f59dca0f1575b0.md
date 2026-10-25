# # Import accounts

Earlier in this section we touched on the point that other CRM systems from which you may be getting your source data might already recognize and compensate for the difference between accounts and contacts. However, if your data is being imported from a simpler contact manager— such as Microsoft Outlook, then the only data available is contact data, and you will need to be a bit creative if you wish to create an account-contact relationship for those records. See the section titled _Export contacts from your current contact manager_ later in this chapter if you require assistance with exporting your data from Microsoft Outlook.

We will assume that the file you exported is already in the proper state for importing.

This is also an opportune time to cleanse your data of duplicate records and other integrity issues. Performing this step before you import the data will help ensure that the end result in SugarCRM is of good quality and allows you to make immediate use of it.

All of these issues must be corrected through external tools. You should not attempt to import your data until you have performed a preliminary analysis of your source files to ensure that these issues are eliminated. Failing to do so only leads to frustration and loss of time.

Another possibility is that your export file includes a multi-line text field. For example, a notes or comments area. Those fields are prime areas for extraneous new line markers, as users would have likely used a carriage return at some point while typing in the field in the other application. Given that a new line marker denotes a new record, the pattern of the file would again be skewed and the source file would not be read correctly.

Other issues are the result of the data itself. For example, if someone inputs a contact name with a comma (,) such as, _Magaña, Angel_, the resulting exported file may include a value that contains an extraneous comma. The extra comma throws off the file's pattern and causes read problems.

For example, it is not uncommon for Microsoft applications to not delimit numeric values. This has the potential of causing problems because it disturbs the pattern as you end up with a line containing some columns with delimited values and other values that are not delimited.