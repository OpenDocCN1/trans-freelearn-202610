Notice that with this example of delimited data, there is no non-data information. Everything is either a row or a data value. And the data is highly structured. Yet, there are still some options that can differentiate one delimited format from another. The first differentiator is how each instance of the data (each row) is separated. Usually at the end of a line, a new line, or a carriage return, or both are used depending on the operating environment in use during the creation of the file.

# Seeing invisible characters

In the preceding example, the new line or carriage return is invisible. How do you see the invisible characters? We will read the same file in Text Wrangler on a Mac (similar full-featured editors such as Notepad++ are available for Windows), where we can use the Show invisibles option (located by navigating to View | Text Display).

![](bbox=[0.28, 0.38, 0.72, 0.45])

Another way to view the invisible characters is to use vi (a command-line text editor) on a Linux system or in the Terminal window on a Mac (this is not available on a Windows machine, by default). The process to view the invisible characters in a file using vi is as follows:

+   1. First, use the command:
+      `vi <filename>`
+   2. Then, type : to enter the vi edit mode.
+   3. Then, type set list and press Enter to view the invisible characters.

The following screenshot shows the end-of-line characters that are revealed by set list in vi, showing line termination symbol $.