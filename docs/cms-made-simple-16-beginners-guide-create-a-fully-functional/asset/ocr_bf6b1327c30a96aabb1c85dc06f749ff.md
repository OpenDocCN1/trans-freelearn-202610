Toolbar 1:
cut,paste,pastetext,pasteword,copy,separators,justifyleft,justifycenter,justifyright,justifyfull,separa

Toolbar 2:
bold,italic,underline,strikethrough,advhr,separators,bullist,numlist,separators,outdent,indent,sep

Toolbar 3:
search,replace

Click here to view toolbar-instructions in TinyMCE-wiki.

7. Click on the Save profile button, and see the additional functions added to the third line of the toolbar, as shown in the following screenshot:

![](bbox=[0.212, 0.4, 0.792, 0.535])

# What just happened?

Firstly, you activated the plugin in the Plugins tab. Activating a plugin means that the plugin is loaded with TinyMCE. But it is not enough to just activate it. In the second step, you had to customize the toolbar of TinyMCE to inform the editor where the buttons for the plugin should be shown.

You saw that a single plugin can provide two buttons. Therefore, there were two words you had to enter in the field Toolbar 3. The first one, search, was for the search button, and the second one, replace, was for the replace function. They must not be used together. You can omit the first or the second one if you do not need that specific functionality.

> Add the plugin removeformat in the Toolbar 3 field. This is a plugin that must not be activated first as it belongs to standard. The plugin allows the removal of any formatting from the text without looking at the HTML.