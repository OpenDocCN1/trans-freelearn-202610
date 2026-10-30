Time to add a link to your container! Follow these steps:

+   1. Add a link to your page and shrink it so that it will fit inside your new container.
+   2. Move the link onto the container with the **Home** icon.
+   3. Remove all text from the link.
+   4. Make sure that the link widget has a transparent background.
+   5. Set the link to navigate to the **Home** page.
+   6. Click **Save** to save your dashboard.

Test your dashboard—the home icon will now take you back to the **Home** page.

Let's now have a look at an introduction to SAQL—your first foray into code in TCRM!

# Beginning with code – an introduction to SAQL

This section will introduce you to coding in TCRM using SAQL.

# Definitions

We'll now look at detailed definitions regarding what SAQL is and when it can be used.

# What is SAQL?

**SAQL** is a runtime query language that enables ad hoc analysis of datasets. A SAQL script consists of a sequence of statements that are made up of keywords (such as filter, group, and order), **identifiers (IDs)**, literals, or special characters. It is **JavaScript Object Notation (JSON)**-based and **Apache Pig Latin (PIGQL)**-familiar. A SAQL query loads an input dataset, operates on it, and outputs the results.

Here is a SAQL example:

```
q = load "My_Dataset";
q = group q by 'Division_Name';
q = foreach q generate 'Division_Name' as 'Division_Name', sum('Amount')/sum('Quantity') as 'sum_Amt/Qty'; q = order q by 'sum_Amt/Qty' desc;
```