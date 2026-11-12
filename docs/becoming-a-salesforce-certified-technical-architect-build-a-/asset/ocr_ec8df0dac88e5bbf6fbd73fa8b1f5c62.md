Document management systems today differ in terms of complexity, features, and scope; from simple standalone applications to enterprise-scale systems that can serve massive audiences. Some of the key capabilities available in today's ECM systems are as follows:

+   - Security and access management, in a granular format
+   - Version control
+   - Comprehensive audit trails
+   - Ability to check-in and check-out the documents, with locks incorporated

As a Salesforce Architect, it is important to understand the capabilities of document management systems and enterprise content management. You must also understand what additional tools are needed to incorporate a full-fledged DMS into your architecture.

Salesforce comes with built-in features that can deliver some DMS capabilities. It is important to understand the capabilities and limitations of each (including availability since some features may retire with time). These features are as follows:

+   - **Files:** This allows users to upload and store electronic documents. Files can be stored privately or shared with others using chatter. Files are stored on the platform and are considered as replacements for the old attachments feature, particularly on lightning experience.
+   - **Salesforce CRM content:** This allows users to create, duplicate, and develop enterprise documents. A good example is a presentation or a flyer. It also allows you to share this content with other internal or external users, such as customers or partners.
+   - **Salesforce knowledge:** This is more like an enterprise knowledge base. It allows users to create and curate knowledge articles and publish them using a controlled workflow. It also allows internal and external users (through communities) to search these knowledge articles and use them in multiple ways (for instance, attach them to a case). Knowledge articles can be organized into libraries, which makes them easier to maintain and share.
+   - **Documents:** These are mostly used to store static files in folders without needing them to be attached to records. This is particularly useful when you need to refer to a static resource (such as a logo) on a Visualforce page, for example.

Most enterprises will already have a DMS of some sort; for example, enterprise-wide applications such as Microsoft SharePoint, Box, Google Drive, One Drive, and others. You will most likely need to integrate with these systems to access and manage external documents within the Salesforce UI rather than create things from scratch.