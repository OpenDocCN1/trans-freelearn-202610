For a text-based save file, the conditions differ from those for a binary save:

+   • Files are larger, except when they are compressed into a .zip file or similar. Then, another transformation step (pack/unpack) is needed.
+   • Data must be transformed from binary representation to text and back, every time the data is loaded and saved.
+   • Domain-specific knowledge of the file format may be required for larger changes, to create a save file from scratch. But for simple value changes, a text editor is the only tool we need.
+   • Corrupted data can be fixed, or the corrupt data elements may be simply deleted from the text file. Better to lose only some data than all of it.
+   • File format updates can be detected by advancing a version number in the file, helping the application to use the correct transformations.
+   • Loading the same file on a computer with a different architecture or operating system is no problem at all. The textual representation is the same, and due to the transformation from text to binary data types, endianness or data type lengths do not matter.
+   • Some caveats still exist for cross-platform usage, like different path separators in Windows and Linux, or the different interpretation of points and commas in the locale settings of the system.
+   • If the configuration needs to be split into multiple files, packing all files into a compressed file is the most common way. By adding all configuration files in a .zip or .tar.gz file, you end up with only a single file, and save some disk space due to the compression.

Using a textual representation for our save file is the way to go. And, by defining a simple file format, it is even possible to create a configuration file by hand.

But, before we start creating a file format on our own, let’s check some available file formats. By using a well-known file format, we can save a lot of time since we don’t have to create the functions to parse and write a file.

# **Choosing a text format to save our data**

We will look at three popular configuration file formats in this section:

+   • INI
+   • JSON
+   • YAML