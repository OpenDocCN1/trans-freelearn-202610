Every time you run an upgrade of CMS Made Simple or any of its modules, you should create a new checksum file. In the admin console, open Site Admin | System verification, and click the Submit button in the Download Checksum File section. This function allows you to generate a checksum file for the current installation and save it on your local computer to be verified later. This should be done just prior to rolling out the website, and/or after any upgrades or major modifications. The integrity of the data can be checked at any later time by comparing the checksum file with the stored files.

Using this function can assist in finding problems with uploads, or exactly what files were modified if your system has been hacked. If you suddenly encounter strange issues and suspect that your system has been attacked, then you should make a system verification first to see whether there are any changed files. If the checksums do not match, then the data was certainly altered (either intentionally or unintentionally).

> The original checksum file from http://dev.cmsmadesimple.org only verifies the core CMS Made Simple and the core modules that are delivered with CMS Made Simple. Any additional modules that you've added are not verified.

You can see the errors, as shown in the following screenshot:

```
1 Files Not found
Files Not found:
./tmp/cache/SITEDOWN

2 Files failed md5sum check:
./tmp/templates_c/index.html
./tmp/cache/index.html
```

According to the preceding explanation, these errors are not critical. If you get other failures, it is up to you to interpret what has happened. If you are sure that you have not changed any files, then somebody else has done it to damage your website. In this case, download the original release from CMS Made Simple, extract it, and replace the changed files with the original.

Replacing the files is the first step. If your system has been hacked, it means that there is still a security hole that can be used again and again to damage your system. Please read and implement the following sections to see how you can protect your system.