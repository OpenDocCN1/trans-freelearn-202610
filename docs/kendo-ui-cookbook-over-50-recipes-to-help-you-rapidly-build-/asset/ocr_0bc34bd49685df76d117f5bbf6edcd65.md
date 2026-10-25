# # Kendo UI File Uploader

In this chapter, we will cover the following recipes:

+   - Using the file uploader to upload files to the server
+   - Uploading files to the server asynchronously
+   - Listening to file upload events

# ## Introduction

The Kendo UI library provides a file uploader widget that can be used to upload multiple
files to the server. On modern browsers such as Chrome, Safari, Firefox, and Opera, the
files can be uploaded asynchronously using the HTML5 File API. The enhancements to the
XmlHttpRequest object in HTML5, such as progress tracking, are also available. On legacy
browsers, it falls back and uploads the file synchronously. In this chapter, we will see the various
ways in which the file uploader widget can be used to upload files to the server, showing the
progress of the file upload process and the options available to customize the widget.

# ## Using the file uploader to upload files
to the server

In this recipe, we will look at how the file uploader widget can be used to upload multiple files
to the server synchronously.