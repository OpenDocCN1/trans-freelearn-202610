# Invoking a web script from client-side JavaScript

The client-side JavaScript control file can be associated with components in Alfresco Share. If you need to make a repository web script call, you can do this from the client-side JavaScript control files generally located at tomcat\webapps\share\components. There are different ways you can make a repository web script call using a YUI-based client-side JavaScript file. The following are some of the ways to invoke web script from client-side JavaScript files. References are also provided along with each of the ways to look in the Alfresco out-of-the-box implementation to understand its usage practically:

+   - Alfresco.util.Ajax.request: Take a look at tomcat\webapps\share\components\console\groups.js and refer to the _removeUser function.
+   - Alfresco.util.Ajax.jsonRequest: Take a look at tomcat\webapps\share\components\documentlibrary\documentlist.js and refer to the onOptionSelect function.
+   - Alfresco.util.Ajax.jsonGet: To directly make a call to get web script, take a look at tomcat\webapps\share\components\console\groups.js and refer to the getParentGroups function.
+   - YAHOO.util.Connect.asyncRequest: Take a look at tomcat\webapps\share\components\documentlibrary\tree.js and refer to the _sortNodeChildren function.

> In alfresco.js located at tomcat\webapps\share\js, the wrapper implementation of YAHOO.util.Connect.asyncRequest is provided and various available methods such as the ones we saw in the preceding list, Alfresco.util.Ajax.request, Alfresco.util.Ajax.jsonRequest, and Alfresco.util.Ajax.jsonGet can be found in alfresco.js. Hence, the first three options in the previous list internally make a call using the YAHOO.util.Connect.asyncRequest (the last option in the previous list) only.

# Calling a web script from the command line

Sometimes while working on your project, it might be required that from the Linux machine you need to invoke a web script or create a shell script to invoke a web script. It is possible to invoke a web script from the command line using cURL, which is a valuable tool to use while working on web scripts.