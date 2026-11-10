# SVN

SVN or Subversion, allows multiple developers to work on the same project. The project is usually maintained at the server, and the ones that are working on it do a checkout (creating a local copy) of the files from the server. It is also possible to commit the modified files back to the server. In order to perform any operation, there is need for connecting to the server.

SVN, in contrast to CVS, can track not only file change history, but all the changes regarding the shared projects, such as allowing the addition, deletion or removal of all resources without creating new items, and losing information about deleted ones. A commit operation in SVN is absolutely atomic, that is, the repository will never reach an inconsistent state, and versioned properties' support is available.

Eclipse does not come with an SVN client integrated by default. It is necessary to install an additional plugin, namely the **Subversive project**. Nonetheless, it is maintained as an Eclipse project in the Eclipse portal. Another option is to make use of the **Subclipse project** (http://subclipse.tigris.org/). The Subversive plugin aims to integrate the Subversion version control system with the Eclipse platform. The plugin allows the possibility of working with projects stored in Subversion repositories directly from the Eclipse workbench, in a similar fashion as other Eclipse version control providers, such as CVS and Git.

The project was started in 2005 by the Polarion Company as an open source project and was later migrated under the Eclipse umbrella as an official Eclipse project in 2007. Nowadays, the Subversive project consists of the Subversive plugin for Eclipse, developed as an Eclipse open source project, along with the Subversion connectors, developed as an external open source project.

Subversive has been designed to be used as a full-featured SVN client, so it is possible to update, commit, merge changes, work with SVN properties, view change history, and perform other operations with SVN directly from the Eclipse environment. Moreover, Subversive evolves together with the Subversion project to provide Eclipse users with the features that appeared in the new versions of the SVN implementation.

The **Juno version** of the plugin can be installed using the update-site http://download.eclipse.org/technology/subversive/1.0/update-site-1.0.1/.