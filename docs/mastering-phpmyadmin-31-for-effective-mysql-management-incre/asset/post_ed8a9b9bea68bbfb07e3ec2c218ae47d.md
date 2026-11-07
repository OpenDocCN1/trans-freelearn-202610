
## Microsoft Word 2000

This export format directly produces a .doc file suitable for all software that understand the Word 2000 format. We find options similar to those in the Microsoft Excel 2000 export, and a few more. We can independently export the table's Structure and Data.

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_154_0.png)

Note that, for this format and the Excel format, we can choose many tables for one export. However, unpleasant results happen if one of these tables has non-textual data. Here are the results for the author table:

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_154_1.png)

## LaTeX

LaTeX is a typesetting language. phpMyAdmin can generate a .tex file that represents the table's structure and/or data in a sideways tabular format. Note that this file is not directly viewable, and must be processed further or converted for the intended final media.

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_155_0.png)

The available options are:

- **Include table caption**: Display captions in the tabular output.
- **Structure and Data**: The familiar choice to request structure, data, or both.
- **Table caption**: The caption to go on the first page.
- **Continued table caption**: The caption to go on, page after page.
- **Relations, Comments, MIME-type**: Other structure information we want as output. These choices are available if the relational infrastructure is in place (see Chapter 11 for details).

## XML

This format is very popular these days for data exchange. Choosing **XML** in the **Export** interface yields no choice for options. What follows is the output for the **author** table.

```
<?xml version="1.0" encoding="utf-8" ?>
<!--
-
- phpMyAdmin XML Dump
- version 3.1.1
- http://www.phpmyadmin.net
-
- Host: localhost
- Generation Time: Dec 21, 2007 at 01:13 PM
- Server version: 5.0.67
- PHP Version: 5.2.6
-->

<!--
- Database: `marc_book`
-->
<marc_book>
    <!-- Table author -->
    <author>
        <id>1</id>
        <name>John Smith</name>
        <phone>+01 445-789-1234</phone>
    </author>
    <author>
        <id>2</id>
        <name>Maria Sunshine</name>
        <phone>333-3333</phone>
    </author>
</marc_book>
```

## Native MS Excel (pre-Excel 2000)

phpMyAdmin offers an experimental module to export directly into a **.xls** format—the native spreadsheet format understood by MS Excel and OpenOffice Calc. When this support is activated (more on this in a moment), we see a new export choice, **Native MS Excel**, and the same options as those for Microsoft Excel 2000.

We can optionally put our field names in the first row of the spreadsheet, with **Put fields names at first row.**

This functionality relies on the PEAR module **Spreadsheet_Excel_Writer**, which is currently at version 0.9.1 and generates Excel 5.0 format files. This module is documented at http://pear.php.net/package/Spreadsheet_Excel_Writer, but the complete installation in phpMyAdmin's context is documented here:

- 1. Ensure that the PHP server has PEAR support. (The pear command will fail if we do not have PEAR support.) PEAR itself is documented at http://pear.php.net.
- 2. If we are running PHP in safe mode, we have to ensure that we are allowed to include the PEAR modules. Assuming that the modules are located under /usr/local/share/pear, we should have the line safe_mode_include_dir = /usr/local/share/pear in php.ini.
- 3. We then install the module with pear -d preferred_state=beta install -a Spreadsheet_Excel_Writer (because the module is currently in beta state). This command fetches the necessary modules over the Internet and installs them into our PEAR infrastructure.
- 4. We need a temporary directory, under the main phpMyAdmin directory, for the .xls generation. It can be created on a Linux system with mkdir tmp ; chmod o+rwx tmp.
- 5. We set the $cfg['TempDir'] parameter in config.inc.php to './tmp'.

We should now be able to see the new **Native MS Excel data** export choice.

## Open document spreadsheet

This spreadsheet format is a subset of the open document (http://en.wikipedia.org/wiki/OpenDocument), which was made popular with the OpenOffice.org office suite. We need to choose only one table to be exported in order to have a coherent spreadsheet. Here is our author table, exported into a file named author.ods, and subsequently looked at from OpenOffice:

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_157_0.png)

## Open document text

This is another subset of the open document standard, this time oriented towards text processing. The available options are as shown in the following screenshot:

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_158_0.png)

Our author table is now exported and viewed from OpenOffice.

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_158_1.png)

## YAML

YAML stands for YAML Ain't Markup Language. YAML is a human-readable data serialization format; its official site is http://www.yaml.org. This format has no option that we can choose from within phpMyAdmin. Here is the YAML export for the author table:

```
1:
  id: 1
  name: John Smith
  phone: +01 445-789-1234

2:
  id: 2
  name: Maria Sunshine
  phone: 333-3333
```

## CodeGen

This choice might some day support many formats related to code development. Currently, it can export in NHibernate Object-relation mapping (ORM) format. For more details, please refer to http://en.wikipedia.org/wiki/Nhibernate.

## Texy! text

Texy! is a formatting tool (http://texy.info/en/) with its own simplified syntax. Here is an example of export in this format:

```
===Database marc_book

== Table structure for table author
|------|
|Field|Type|Null|Default|
|------|
|/**id**/|int(11)|Yes|NULL
|name|varchar(30)|Yes|NULL
|phone|varchar(30)|Yes|NULL

== Dumping data for table author
|1|John Smith|+01 445 789-1234
|2|Maria Sunshine|333-3333
```

## Table exports

The Export link in the Table view brings up the export subpanel for a specific table. It is similar to the database export panel, but there is no table selector. However, there is an additional section for split exports before the Save as file subpanel.

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_160_0.png)

## Split-file exports

The Dump 2 row(s) starting at record # 0 dialog enables us to split the file into chunks. Depending on the exact row size, we can experiment with various values for the number of rows to find how many rows can be put in a single export file before the memory or execution time limits are hit in the web server. We could then use names like book00.sql and book01.sql for our export files.

## Selective exports

At various places in phpMyAdmin's interface, we can export the results that we see, or select the rows that we want to export.

## Exporting partial query results

When results are displayed from phpMyAdmin (here, the results of a query asking for the books from author_id 2), an Export link appears at the bottom of the page.

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_161_0.png)

Clicking on this link brings up a special export panel containing the query on the top, along with the other table export options. An export produced via this panel would contain only the data from this result set.

> The results of single-table queries can be exported in all the available formats, while the results of multi-table queries can be exported only in the CSV, XML, and LaTeX formats.

## Exporting and checkboxes

Anytime we see the results (when browsing or searching, for example), we can check the boxes beside the rows that we want, and use the With selected: export icon to generate a partial export file with just those rows.

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_162_0.png)

## Multi-database exports

Any user can export the databases to which he/she has access, in one operation.

On the homepage, the Export link brings us to the screen shown on the following page. This has the same structure as the other export pages, except for the databases list.

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_162_1.png)

> Exporting large databases may or may not work. It depends on their size, the options chosen, and the web server's PHP component settings (especially memory size and execution time).
