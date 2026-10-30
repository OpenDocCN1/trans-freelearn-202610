+   - `If file exists / Wait for file`: These can be used to check whether a file exists/does not exist or whether a file is created/deleted.
+   - `Copy / Move / Delete / Rename files`: Operations for one or more files.
+   - `Read / Write text to file / CSV file`: Has been used in the previous example (CMD sessions).
+   - `Get file path`: This action produces several variables containing the root path, the directory, the file name with and without extension, and the extension itself.
+   - `Get temporary file`: Unlike the name suggests, a temporary empty file is created here that can be used for further purposes.
+   - `Convert file to Base64 or back/t4o binary or back`: File conversion might be necessary in some cases to transfer files over a specific protocol and back to make it readable again. Base64 encoding, for example, encodes binary data as printable text.

Another action group worth mentioning in this context is **Compression**, which deals with one or more files that need to be compressed or decompressed. This is the same functionality that is also available when we use Windows Explorer and right-click any file and choose **Compress to Zip**.

Now let’s look at an example of how these actions can be used to create a flow to copy daily backups of specific files to a network share. Conceptually, the flow should perform the following actions: