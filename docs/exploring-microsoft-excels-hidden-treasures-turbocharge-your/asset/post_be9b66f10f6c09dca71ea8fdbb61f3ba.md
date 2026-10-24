

Microsoft is using **Excel for the Web** and Excel Mobile as test platforms for new features such as this, so it’s likely, at some point in the future, we’ll see this **Show Changes** feature make its way into the desktop versions of Excel. For instance, Excel Mobile has offered the ability to convert pictures of text and numbers into editable data for several years now. As of this writing, that feature is being beta tested in the desktop versions of **Microsoft 365**.

## Summary

In this chapter, first, you saw how the **Quick Access Toolbar** is evolving in **Microsoft 365** and how to unhide the toolbar if needed. The **Quick Access Toolbar** allows you to create up to 44 custom keyboard shortcuts in Excel for Windows, but if you’re not aware of the nuance, you might be baffled why, let’s say, a heart or spade appears in your worksheet cell instead of carrying out a command. We always _hold down_ the `Ctrl` key when executing a shortcut such as `Ctrl + Z`, but we must use an alternate behavior for the `Alt` key. Remember to _tap_ the `Alt` key if you want to your number pad to execute a shortcut. You have complete control over the order of your icons, which enables you to prioritize the keyboard shortcuts.

We explored ways to create shortcuts for **Ribbon** commands that don’t have shortcuts, and we dug into the **Commands Not in the Ribbon** area to unearth commands that can streamline repetitive tasks. You can add commands to your default **Quick Access Toolbar**, or you can create workbook-specific **Quick Access Toolbars**.

Sometimes, there just is not a command for a task that you want to streamline, so we explored how to use the **Macro Recorder** feature to automate rote tasks. If you invest a lot of time curating your **Quick Access Toolbar**, you will likely want to back it up, perhaps transfer it to a new computer, or share your handiwork with a colleague. The transfer is easy to accomplish in Excel for Windows. Some of the techniques I shared in this chapter are not available in Excel for macOS, but the **Quick Access Toolbar** can still be a valuable resource on that platform.

Then, we looked at how to restore legacy features in Excel—Microsoft’s term for features they wish to retire but that you might wish to keep using. As you saw with **Full Screen**, replacement functionality Microsoft adds to Excel isn’t necessarily better than what it replaces. Sometimes, the replacement functionality, such as for Track Changes, doesn’t even exist in the desktop version yet, as evidenced by the **Show Changes** feature in Excel for the Web.

In Chapter 4, _Conditional Formatting_, I’ll show you how you can enliven data in your workbooks with color, cell formatting, and even in-cell charts and icons.

## Conditional Formatting

In this chapter, I’ll show you the wonders of Excel's **Conditional Formatting** feature that can help you enliven your data with color, charts, or icons. You can, of course, use all three at once, but usually, you are more when it comes to data visualization. **Conditional Formatting** is a live overlay for worksheet cells that dynamically applies formatting such as font color, cell color, borders, and number formatting, based upon the cell contents matching specific criteria. The built-in **Conditional Formatting** rules eliminate the need to manually color-code data, and you can extend this functionality in limitless ways by crafting custom rules. Along the way, I’ll point out nuances that frustrate even the most experienced spreadsheet users.

In this chapter, I’ll cover the following:

- Formatting versus Conditional Formatting
- Highlight Cell Rules
- Top and Bottom Rules
- Data Bars
- Color Scales
- Icon Sets
- Wingdings symbols
- Custom rules
- Managing Conditional Formatting rules
- Removing Conditional Formatting
- Conditional Formatting nuances

By the end of the chapter, you’ll see numerous ways that **Conditional Formatting** can identify and illustrate data based on criteria that you specify. Twenty years ago, Excel had a limit of no more than three **Conditional Formatting** rules per worksheet, but now there is no practical limit to the number of rules that you can apply to a worksheet.

## Technical requirements

Everything in this chapter will work the same in Excel for macOS and Excel for Windows.

At the time of writing, Microsoft has made some changes to the **Conditional Formatting** feature in **Excel for the Web** that will most likely make their way into the desktop versions of Excel. Excel for the Web has emerged as a proving ground for user interface changes that Microsoft is contemplating.

The workbook that I used in this chapter is available for download from GitHub at `https://github.com/PacktPublishing/Exploring-Microsoft-Excels-Hidden-reasures/tree/main/Chapter04`.

## Formatting versus Conditional Formatting

In Excel, you can apply a wide variety of formatting to cells by way of the **Format Cells** dialog box. As you'll see, Conditional Formatting utilizes a subset of the **Format Cells** dialog box, which means you can apply almost all of the same formatting with **Conditional Formatting**. Let's compare the two:

1. Choose **Home** | **Format** | **Format Cells**, or press `Ctrl + 1 (`⌘` + 1 in Excel for macOS).

2. As shown in _Figure 4.1_, the **Format Cells** dialog box has six tabs that allow you to control every aspect of formatting within a worksheet cell:

![](img/0c7a357df7e57e06b6d9f631c219040c_104_0.png)

Figure 4.1 – Border tab of the Format Cells dialog box

3. **Optional**: click on each tab to review the available formatting.
4. Click **Cancel** to close the **Format Cells** dialog box.

Let’s now have a look at the **Conditional Formatting** version of the **Format Cells** dialog box:

1. Choose **Home | Conditional Formatting | New Rule | Use a Formula to Determine Which Cells to Format**, and then click the **Format...** button.

2. The abbreviated version of the **Format Cells** dialog box appears, with four tabs instead of six, as shown in **Figure 4.2**. Note how this version of the **Border** tab has fewer styles than the **Border** tab shown in **Figure 4.1**. Also, certain styles, such as diagonal, cannot be applied.

![](img/0c7a357df7e57e06b6d9f631c219040c_105_0.png)

Figure 4.2 – **Conditional Formatting** version of the Format Cells dialog box

3. **Optional**: Click on each tab to review the available formatting.
4. We’ll make use of this dialog box later in the chapter, so for now, click **Cancel** twice to close the **Conditional Formatting** dialog boxes.

As you can see, **Conditional Formatting** allows you to apply one or more of the following formats to any range of cells in your worksheet:

- Number formatting
- Underline
- Strikethrough
- Font colors
- Bold
- Italics
- Certain cell borders
- Background colors
- Cell patterns
- Fill effects

Conversely, you cannot apply any of the following formats to cells with **Conditional Formatting**:

- Font names
- Font sizes
- Superscript or subscript
- Alignment aspects, such as centering
- Cell border styles and positions
- Protection settings, such as unlocking or hiding cell contents

You can, however, go beyond the Format Cells dialog box by conditionally applying **data bars**, **color scales**, and **icon sets**. Further, you can apply as many **Conditional Formatting** rules to a range of cells as you wish. Let’s see how to apply **Conditional Formatting** to a range of cells, starting with the highlight cells rules.

> Nuance
> Long-time Excel users may recall that Excel 2003 and earlier had a limit of three **Conditional Formatting** rules per cell. There’s no limit to the number of rules that you can apply in Excel 2007 and onward.

## Highlight cell rules

The Home | Conditional Formatting | Highlight Cells Rules submenu offers eight different options. I’ll discuss the first seven here and then cover the **More Rules** command later in the chapter in the *Custom rules* section. I’ll demonstrate the **Greater Than** and **Less Than** rules, and then briefly discuss the other rules, since all are applied in the same fashion. The **Greater Than**-**Less Than**-Between worksheet in the example workbook has three rules applied to cells D6:D25. Let’s first apply the **Greater Than** rule to the **Work Area** section of the worksheet.

