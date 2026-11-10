+   - **Copying Folders** (`<target name="copyFolders" depends="compile_code">`): Copy compiled classes .from the Compiled directory to the following location:
+     `C:/Documentum_Build/Compiled_Code.`
+   - **Creating ZIP file** (`<target name="createzip" depends="compile_code">`): Create a ZIP of compiled classes from the compiled directory at the following location: `C:/Documentum_Build.`

+   3. Copy the build.properties and build.xml files to the same location in a folder on your machine. Execute ANT by running the command ant from your command prompt as shown in figure 25.16.

![](bbox=[0.18, 0.31, 0.82, 0.61])

Figure 25.16: Running ANT from the command prompt

Notice the echo messages shown in the console as ANT executes the various tasks mentioned in the build.xml file. These echo messages serve as a good mechanism for debugging ANT scripts.

This concludes our discussion on migration. Documentum provides other mechanisms for migration as well, such as Dump and Load, Docbase copy, FTP Services (earlier known as FtpIntegrator), etc. Another tool worth mentioning here is DIXI (Documentum Import eXport Interface) developed by Blue Fish Group. This is an XML-based system that helps move content in and out of Documentum repositories. However, a discussion about each of these is not possible within the boundaries of this book.