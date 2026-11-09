After choosing the format for the file, we will be prompted for a filename and location for where to save the file. When we launch the resulting file, either in a text editor or in Excel, we can see that it looks a lot like the Tabula version we saw in the previous section. Here is how our CSV file will look when opened in Excel:

|   | A | B | C | D | E | F |
|---|---|---|---|---|---|---|
|1|All||4-year Privat|4-year Public|2-year Privat|For profit|
|2|38|Prepare stud|23|28|47|63|
|3||members of the workforce|||||
|4|27|Prepare your|48|31|14|8|
|5||citizens|||||
|6|21|Ensure that a|16|24|25|20|
|7||equal access to a college education|||||
|8|4|Contribute to|2|5|8|2|
|9||of their region or locality|||||
|10|4|Conduct rese|5|8|3|1|
|11||scientific, social, and other national|||||
|12||problems|||||
|13|2|Provide cont|2|1|1|3|
|14||of all ages|||||
|15|3|None is Over|4|4|3|2|

At this point, we can use the exact same cleaning routine we used with the Tabula data, where we concatenated the B2:B3 cells into a single cell and then removed the empty rows.

# Summary

The goal of this chapter was to learn how to export data out of a PDF file. Like sediment in a fine wine, the data in PDF files can appear at first to be very difficult to separate. Unlike decanting wine, however, which is a very passive process, separating PDF data took a lot of trial and error. We learned four ways of working with PDF files to clean data: copying and pasting, pdfMiner, Tabula, and Acrobat export. Each of these tools has certain strengths and weaknesses:

+   - Copying and pasting costs nothing and takes very little work, but is not as effective with complicated tables.
+   - pdfMiner/Pdf2txt is also free, and as a command-line tool, it could be automated. It also works on large amounts of data. But like copying and pasting, it is easily confused by certain types of tables.