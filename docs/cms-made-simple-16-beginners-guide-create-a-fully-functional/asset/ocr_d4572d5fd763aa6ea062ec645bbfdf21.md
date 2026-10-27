# System verification

A CMS Made Simple installation consists of its files and a database. The database holds page structure, page content, design, and any module data and can change daily on busy websites. In contrast to the database, files are a kind of static part of the website. They are changed only if you:

+   - Upgrade CMS Made Simple
+   - Upgrade any module
+   - Install new modules
+   - Remove modules from the installation
+   - Intentionally or unintentionally make changes to the files on your own

Sometimes files can be changed by malicious programs or hackers if you do not secure your system enough. So how can you be sure that the system files are not changed and everything is still okay.

CMS Made Simple uses a checksum verification to prove that the files on your web hosting have not been changed. A checksum is a text string computed from a file for the purpose of detecting accidental changes that may occur during its transmission or storage. These checksum strings are generated for each file that originates from the core CMS Made Simple and can be downloaded from http://dev.cmsmadesimple.org/project/files/6. A checksum file is generated for each release of CMS Made Simple.

Depending on your version (base or full, and release), choose the right checksum.dat file, and save it to your local disk. Open it in a text editor to see what is inside.

```
3ad39cc13942818630935b56dc8af619  */index.php
6f8ae75cfde0c5a81dc09f35ea40f7a6  */preview.php
d53ee56d1c123a1ed04326ca3406e744  */favicon_cms.ico
...
```

You will find a long list of checksum strings followed by the filenames and their paths. In CMS Made Simple, you can automatically validate all files in the admin console (**Site Admin | System Verification**). This function will compare the checksum strings of the original files with the files on your installation. Normally, your files should not have any changes if you have not made any upgrades to the system so far.

The only acceptable errors are missing files from the install folder, as you had to delete this directory for security reasons. You may also be told that various index.html files in the tmp folder are missing or invalid. This is not critical. Missing the file SITEDOWN is also not critical.