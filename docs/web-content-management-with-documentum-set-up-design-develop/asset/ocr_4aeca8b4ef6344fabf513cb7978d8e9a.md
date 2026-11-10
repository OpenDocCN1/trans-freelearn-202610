> You can create any number of Site Publishing Configurations for a given Docbase and publish to many websites from the same source Docbase.

2. To create a Site Publishing Configuration, you need to fill in the values under the Info and Advanced tabs. Let us take a look at the Info tab to begin with.

Fill in the values for the Docbase Settings section as follows (refer to figure 21.3):

+   - Configuration Name: A valid name for the Site Publishing Configuration.
+   - Publishing Folder: Choose the Web Cabinet (by clicking on Select Source Folder) from which the content needs to be published. In our Docbase, we created the Test_webCabinet web cabinet. All content created under this Web Cabinet directly or within folders contained under this Web Cabinet will be published.
+   - Version: The lifecycle state of the documents at which the publishing should happen. This corresponds to the r_version_label attribute of content.

In our case, we are publishing all documents that are in the Staging state of their lifecycle. If you do not specify a specific version label, the CURRENT version of content is published by default.

Fill in the values for Target Host Settings section as follows:

+   - Target Host Name: Host name or the IP address of the website host (where SCS Target has been installed).
+   - Target Port: The port number on which SCS Target service listens. The default is 2788.
+   - Connection Type: Connection to SCS Target. This can be raw (Non-secure) or SSL (Secure)
+   - Target Root Directory: This is the webroot on the website host where published files will be placed. If the designated file structure does not exist on the host, Documentum creates it while publishing.