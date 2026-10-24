

# Nuance

Images that you place in the **Header** section will appear near the top of your printout, while images that you place in the **Footer** section will appear near the bottom of the page. You cannot place an image in the center of the printed page in this fashion unless the image is large enough to span the entire printed page.

4. Select a section and then click the **Insert Picture** button, which is the second button from the right, as shown in **Figure 1.18**:

![](img/0c7a357df7e57e06b6d9f631c219040c_42_0.png)

Figure 1.18 – Header dialog box

5. Make a choice from the **Insert Pictures** dialog box, which asks whether you want to choose a file from your local drive or an online resource.

6. Once you select an image, a **&[Picture]** placeholder will appear in the section you chose, as shown in **Figure 1.18**.

7. Optional: Click **Format Picture**, which is the last button on the right, to change the size of the picture. You may also wish to click on the **Picture** tab and change **Color** to **Washout** in the **Image** control section.

8. Click **OK** as needed to close any open dialog boxes.

9. Choose **File | Print** to display a print preview, because if you choose **View | Page Layout** to enter **Page Layout** mode, any frozen worksheet panes will become undone. You can return to the **Page Setup** dialog box and change the settings if needed, such as shrinking the dimensions to prevent an image from overrunning the printed page.

To remove headers or footers, choose Page Layout | Print Titles, activate the Header/Footer tab, and then choose (none) from the top of the corresponding drop-down list.

> Nuance
> Page Layout | Background offers a third approach for creating a watermark. The difference is that the image will repeat throughout Excel’s entire grid, and there is no way to edit the image. If you add an image in this fashion, the Background command toggles to Delete Background.

Any worksheet that has vital information in a watermark, header, or footer, such as CONFIDENTIAL or INTERNAL USE ONLY, should be considered inaccessible because assistive technology cannot access those areas of Excel. Any such information should also be repeated in cell A1, where it can be accessed by assistive technology but also be readily visible to all users of the worksheet.

Let’s now see how color can have an impact on the accessibility of your spreadsheets.

# Working carefully with color

Accessibility standards call for colors to have sufficient contrast between background fill and fonts used within cells. One sure-fire way to ensure proper contrast is to use a black background with white text or light shades of gray. Black text on a white background is accessible as well. Conversely, let’s say blue text on a red background can be difficult for anyone to read, much less anyone with vision impairments or color-blindness.

Further, standards call for any indicators in a spreadsheet that are represented by color-only to also have supportive text, as shown in Figure 1.19. As much as 8% of the world’s male population and 0.5% of the female population is color-blind. That means if you work with say nine other people, there’s a good chance at least one person may be color-blind.

![](img/0c7a357df7e57e06b6d9f631c219040c_43_0.png)

Figure 1.19 – An inaccessible list versus an accessible list

The list on the left only uses color to find the status of each project, such that anyone, no matter the level of vision they have, may find themselves struggling to make sense of the data, at least at first. Conversely, the list on the right pairs the color and text together, so that all users can decide the status of each project at once. I discuss how to automate color coding based upon cell contents in Chapter 4, Conditional Formatting, along with an approach where you can combine color coding with cell icons to provide an additional means for identifying types of data.

Let’s now see how the Table feature can improve accessibility within a worksheet.

# ## Using the Table feature

I talk extensively about the Table feature in Chapter 7, Automating Tasks with Tables, so I won’t go into much detail here, but the Table feature is one of the best ways to improve the accessibility of a worksheet. First, you cannot use merged cells within a Table; any commands or options related to merged cells are disabled when your cursor is within a Table.

> > Nuance
> > When you convert a range of cells into a Table, any merged cells within the list will be automatically unmerged because merging cells is not compatible with the Table feature.

Enabling the Header Row and First Column options, as shown in Figure 1.20, can particularly improve accessibility for all:

![](img/0c7a357df7e57e06b6d9f631c219040c_44_0.png)

The Header Row allows you to place meaningful titles in the top row of a list. The titles move up into the worksheet frame when you scroll down past the first row of a Table. Filter arrows appear automatically in the Header Row to enable users to easily collapse the list down to just records of their choice. First Column makes the text in the first column bold but can also be used to help users of assistive technology know that they’re starting out in the first column rather than landing unexpectedly in the middle of a Table. Finally, assigning a meaningful name to the Table by way of Table Design | Table Name helps all users understand at once what type of data is contained within the Table. It is best to also supply a description of the data above the Header Row. As I discuss later in the book, Table Names supply an effortless way for all users of the spreadsheet to be able to jump directly to a list of data by selecting the Table Name from the Name box.

Leaving the default Table Names in place, such as Table1, Table2, Table3, and so on, quickly makes it difficult to know what data is where and cuts off the ability to move purposefully to a list of data anywhere in the workbook.

Let’s now see how you can easily find potential accessibility challenges in any workbook.

# # Accessibility Checker feature

The Accessibility Checker feature, available in Microsoft Excel and other Microsoft Office programs, can review your workbooks and supply feedback on changes you can make to improve the accessibility of your spreadsheets. You can launch the Accessibility Checker feature in one of three ways:

- Choose Review | Check Accessibility.
- In Excel for Windows, choose File | Info | Check for Issues | Check Accessibility.
- Click the Accessibility button in Excel’s status bar, which displays the message Accessibility: Investigate, as shown in Figure 1.21, when one or more potential accessibility issues have been noted, or Accessibility: Good to Go when no issues have been found.

Any of these choices will display in the Accessibility task pane shown in Figure 1.21:

![](img/0c7a357df7e57e06b6d9f631c219040c_45_0.png)

Figure 1.21 – Accessibility Checker

The Accessibility Checker feature has three levels of feedback and a Ribbon tab::

- **Errors** are content that will be exceedingly difficult or impossible for disabled users to use. Situations can include negative numbers formatting in red and information rights restrictions.
- **Warnings** are triggered by content that will be difficult for disabled users to use. Situations can include worksheets with default names, and insufficient contrast between font color and cell fill color, such as dark gray letters on light gray cell fill.
- **Tips** are triggered by content that could be better organized to improve the ease of use for disabled users.

> Nuance
> **Accessibility Checker** is an imperfect feature and may flag issues that seem immaterial while blithely ignoring blatant accessibility issues that you can see in plain sight. Similarly **Spell Check** won't inform you if you've used the word *principle* when you should have used *principal*. With both spelling and accessibility issues, you must trust but verify that everything is in order. The **Accessibility Checker** does offer a **Ribbon** comprised of tools that can help adjust formatting, assign names, along with other accessibility features.

Now, let's look at a hidden tool in Excel that you can use to annotate accessibility issues that you plan to clear up in your workbooks.

# # Accessibility Reminder add-in

The **Accessibility Reminder** add-in is a free tool that makes it easy to add comments to spreadsheets to call attention to accessibility issues. To install this add-in, follow these steps:

1. Choose **Insert** | **Get Add-ins**.
2. Type *Accessibility* in the **Search** field and then press *Enter*.
3. Click the **Add** button next to **Accessibility Reminder** and then click **Continue**.

A new **Accessibility Reminder** tab appears in the **Ribbon**, as shown in **Figure 1.22**.

![](img/0c7a357df7e57e06b6d9f631c219040c_46_0.png)

Figure 1.22 – Accessibility Reminder **Ribbon** tab

The **Accessibility Reminder** tab offers three commands:

- **Add Accessibility Comment**: Adds a generic comment that the file has accessibility issues.

