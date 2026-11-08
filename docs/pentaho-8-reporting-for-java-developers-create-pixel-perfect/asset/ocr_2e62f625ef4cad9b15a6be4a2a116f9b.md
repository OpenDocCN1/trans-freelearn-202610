# Understanding the serialized report format

In the previous chapters, you have already seen the format of Pentaho reports, saved as `.prpt` bundle files. The `.prpt` format is a ZIP file format that includes all the necessary resources to render a report, also referred to as a bundle. The `.prpt` bundle file contains a set of XML files that are crucial to render a report, as well as additional resources, such as embedded images and subreports. This format is based on the OpenDocument format.

This section goes into detail about each of the primary files that make up a report, along with providing a simple example of a report written by hand. With the knowledge of the underlying file format, it is possible to generate reports outside of the Java environment.