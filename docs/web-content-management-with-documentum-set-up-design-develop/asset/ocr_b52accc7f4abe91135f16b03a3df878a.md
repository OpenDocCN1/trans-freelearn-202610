# 4.4 How do you Enter Content in the System?

**Web Publisher** is one of the simplest tools to enter content in the Documentum system and forms the entry point into a WCM system. It is a web-based client tool that connects to the Content Server Docbase and assists content authors, managers, and administrators in creating, managing, and administering content for a business site.

# 4.4.1 Web Publisher

The good part about using Web Publisher is that it simply requires a one-time setup of the Web Publisher system and the business-specific customized files on an application server. The Web Publisher end users (or clients) need not perform any installation on their laptops/desktops and can simply access the Web Publisher URL via a range of supported browsers.

The dmc1.ini file on the application server host where Web Publisher has been installed points to the correct DocBroker and port. It is through this that the connection to the Docbase is made.

Sample dmc1.ini file:

```
[DOCBROKER_PRIMARY]
host ==<<Your DocBroker host Name>>
port =1489 # default port
```

The wide range of operations available in Web Publisher is not just limited to creating or managing content for the website. Web Publisher is an integral part of the Documentum WCM architecture and has an intuitive GUI that allows:

+   - Setting up groups, users, and roles for the end users of Web Publisher
+   - Creation of intuitive categories or taxonomies, using which business users can create classified content for their organization.
+   - Creation of delivery (web) cabinets from where the created content can be published out of the Documentum system.
+   - Setting up security for Docbase objects by attaching Permission sets (ACLs) to the objects in question.
+   - Promoting and demoting objects in Lifecycle states. Using Web Publisher's 'Promote', 'Demote' and 'Power promote' features, one can push a document to the next state(s) in its lifecycle or demote it to its previous state(s).
+   - Creation of content templates and associated rules and presentation files, so that users can simply use the templates to create content without learning any web scripting technologies.
+   - Getting the content items reviewed and approved by other users by pushing them to workflows. It is worth mentioning that Documentum Web Publisher provides some out-of-the-box workflows available for use. Examples: Request new content, submit to website workflow, etc.
+   - Integration with utility services and tools like automatic property extraction, Content Intelligence services, Inter-Enterprise workflow services, In-Context editing, eWebEditPro Rich Text Editor, etc.