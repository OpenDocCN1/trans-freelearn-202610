# # Disaster Recovery and File-Related Prompts

If the repair doesn’t work, carry out the preceding steps again, but this time, click on **Extract Data** in Figure 2.15 instead of **Repair**. When it works, **Extract Data** discards all formatting in the workbook— meaning fonts, colors, number formatting, borders, and more—and only keeps the formulas and the data. In my experience, if the first option fails, **Extract Data** will fail too, but it never hurts to try.

If the **Repair** option within Excel doesn’t work, the next steps that I will try are listed as follows:

+   • Opening the damaged workbook in Excel for macOS, which can sometimes open files that the Windows version can’t.

+   • Download and install the free **LibreOffice** suite. Sometimes, the Calc app within **LibreOffice** can open damaged Excel workbooks.

> > **Tip**
> > Additionally, you can use **LibreOffice** if you need to open a **Lotus 1-2-3** file so that you can convert it into an Excel workbook.

+   • Write a formula in a new workbook that links back to the damaged workbook. If the workbook is named `Damaged File.xlsx` and is stored in `E:\Documents`, then enter `= 'E:\Documents\Damaged File.xlsx'!A1` into cell **A1** of a blank worksheet. Copy the formula down and across to fill the approximate area you feel the damaged worksheet spans. Note that you can reference a sheet name if you know it, or you can omit the sheet name to pull data from the first worksheet only.

Now, let’s see a proactive step that you can take to improve the stability of your workbooks, especially those that have been passed around among multiple users.

# ## Removing excess formatting

Sometimes, you can forestall the need to repair an Excel workbook by removing excess formatting. Such formatting arises when users select an entire column and apply number formatting, fonts, colors, and more to more than one million cells. When you start doing things a million times here and a million times there, it’s easy for a workbook to become bloated and unstable. Whenever possible, only apply formatting to the specific cells that are needed. Fortunately, you can overcome past transgressions from yourself or others. **Microsoft 365 Apps for Enterprise** offers an **Inquire Add-in** that includes a **Clean Excess Cell Formatting** feature that can slim down bloated workbooks.

> > **Nuance**
> > **Microsoft 365 Apps for Enterprise** is considered the business version of Excel, as opposed to the **Home and Family** version of **Microsoft 365**. If you’re not clear about which version of Excel you have, choose **File**, and then **Account**. If **Microsoft 365 Apps for Enterprise** appears on the right-hand side, then you have access to the **Inquire Add-in** option.