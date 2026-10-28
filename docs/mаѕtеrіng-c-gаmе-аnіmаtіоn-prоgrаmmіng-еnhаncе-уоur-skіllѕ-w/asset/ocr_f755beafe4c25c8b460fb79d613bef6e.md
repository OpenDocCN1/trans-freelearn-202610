Loading the same data was as easy and fast as saving it: read the data from the storage device to the computer’s memory (again, byte by byte) and interpret it as the same internal data types that were used while saving.

Let’s take a look at some of the advantages and disadvantages of binary data:

+   - **Advantages:**
+     - Files are small.
+     - Saving and loading the data can be done by just copying the data, leading to a higher speed for save and load operations.
+   - **Disadvantages:**
+     - To change the data outside the application, special knowledge is needed.
+     - Corrupted data could cause unpredictable side effects or crashes.
+     - Updates to the save file format may be hard. *Magic bytes* are needed to find the actual version and to map the loaded data to the correct internal data types.

Binary data may not be portable across architectures due to different endianness. For this reason, it is generally good advice to avoid binary save files unless absolutely necessary. We can still read CONFIG.SYS and AUTOEXEC.BAT files created on MS-DOS systems 30 years ago, but the binary save files from spreadsheet calculators or word processors of the same time are unusable, at least without the correct tools or hard work to reverse-engineer the file format. Outside of well-documented and standardized formats, like pictures or sound files, saving binary data will cause trouble, as you may not be able to open the binary files on a different operating system, or even just a newer version of the same system. Also, loading the file on a different CPU architecture, bit width, or endianness will most probably fail due to a different memory layout of the data types.

Since CPU time and storage space are no longer limited, the advantages of text formats now clearly outweigh those of binary saves.

# Saving and loading textual data

After more and more CPU power, network bandwidth, and storage space became available, textual formats started to become the first choice to save data. Developing code is much easier when you can create or adjust the saved files with a simple text editor, or when you can find errors by printing the text lines to log files.