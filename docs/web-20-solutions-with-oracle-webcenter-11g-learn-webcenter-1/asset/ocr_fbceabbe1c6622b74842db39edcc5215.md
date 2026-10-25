Some of the common challenges we have found with implementing forums for the enterprise have been related to the following:

+   1. Integration into the existing technology stack
+   2. User and group administration using existing LDAP
+   3. Support
+   4. Cultural adoption

Many of the most popular forum software solutions have been based on PHP and MySQL. Not long ago, we implemented the open source phpBB solution for an Oracle Fusion Middleware customer. Most of the implementation time and cost was spent in integrating with the existing Portal infrastructure and identity management components.

With Discussions, Oracle has successfully minimized the technical level of effort required during implementation by addressing the main technical challenges (numbers 1 and 2 above). And with formal support offered, as well as the general idea that forums integration using Discussions will not be unique for each implementation, organizations can feel more comfortable about viable long-term support. Given how well Discussions addresses the first three challenges, we now view the cultural adoption as the largest challenge, which can be overcome with effective training and strategic management decisions. Forums in general have become increasingly popular within organizations and Oracle has implemented an intuitive end-user approach.

The Discussions Server is installed as part of Oracle WebCenter installation. It can be accessed directly at the default location `http://hostname:8890/owc_discussions`. While Discussions in itself is a useful forum tool, the real power comes from the Discussions Service that enables us to embed, view, and interact with the forums as part of custom WebCenter applications where forums can be used as a means to obtain and share information by the enterprise from within the current application being used.

A **Wiki** is a website that allows a group of editors to easily create web pages in a collaborative manner using simple markup language. The markup language helps to organize and format the web pages. Many corporations use Wikis to create documentation internally. Each employee of the company has easy access to edit the information within the wiki, and can therefore incorporate new information for the entire community.