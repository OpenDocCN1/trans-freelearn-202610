On the left-hand side, you see English words and phrases that have to be translated. Translations are made on the right-hand side of the window. If the translation has not been made, then both columns are identical and contain English. You can replace the English text in the right column by the translation into your language and submit. That's all! The more the users participate in translations, the better the translation will be.

> If you want to participate and your language doesn't exist in the Forge, create a project and inform the CMS Made Simple Forge admin, then the language will be added to the Translation Center.

The translations made here will be available to all users with the next release of CMS Made Simple or with the next release of the core or third-party modules. If you need the translation right now or if you would just like to customize the existing language to your needs, then you have to add or change the language files manually on your server.

# Translating modules in your installation

Language files for the modules are always saved in the same folder `/modules/MODULE_NAME/lang/ext`. Only the default English translation is placed directly in `modules/MODULE_NAME/lang` in the file `en_US.php`. Open the file to see how it is organized.

All language files are built the same way for each language. They consist of a single PHP array `$lang`.

```
$lang['key'] = 'Custom language translation';
```

Search for the file with the required language in `/modules/MODULE_NAME/lang/ext`, and open it for editing. If the file is not there, then it means that the module has not been translated yet. In this case, you can just copy the file `en_US.php` into the folder `/modules/MODULE_NAME/lang/ext` and rename it to the language you need. For German, you would name the file `de_DE.php`, for French `fr_FR.php`, and so on.

The file can be edited with a simple text editor like Notepad or WordPad. However, this way of translation requires some basic PHP knowledge. Be aware that the custom changes made in these files may be overwritten with the next release of the module or CMS Made Simple, so be sure that you create a backup file with the changes made somewhere apart from your installation.

Let's see how you can translate your own module, **BWProducts** (created in Chapter 8, *Creating Your Own Functionality* with **CTLMakerModule**), into other languages.