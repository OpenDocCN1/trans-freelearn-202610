
## Downloading the example code

You can download the example code files for all Packt books you have purchased from your account at http://www.packtpub.com. If you purchased this book elsewhere, you can visit http://www.packtpub.com/support and register to have the files e-mailed directly to you.

You can download the code files by following these steps:

- 1. Log in or register to our website using your e-mail address and password.
- 2. Hover the mouse pointer on the SUPPORT tab at the top.
- 3. Click on Code Downloads & Errata.
- 4. Enter the name of the book in the Search box.
- 5. Select the book for which you’re looking to download the code files.
- 6. Choose from the drop-down menu where you purchased this book from.
- 7. Click on Code Download.

Once the file is downloaded, please make sure that you unzip or extract the folder using the latest version of:

- WinRAR / 7-Zip for Windows
- Zipeg / iZip / UnRarX for Mac
- 7-Zip / PeaZip for Linux

The code bundle for the book is also hosted on GitHub at https://github.com/PacktPublishing/Enterprise-PowerShell-Scripting-Bootcamp. We also have other code bundles from our rich catalog of books and videos available at https://github.com/PacktPublishing/. Check them out!

## Downloading the color images of this book

We also provide you with a PDF file that has color images of the screenshots/diagrams used in this book. The color images will help you better understand the changes in the output. You can download this file from https://www.packtpub.com/sites/default/files/downloads/EnterprisePowerShellScriptingBootcamp_ColorImages.pdf.

## Errata

Although we have taken every care to ensure the accuracy of our content, mistakes do happen. If you find a mistake in one of our books – maybe a mistake in the text or the code – we would be grateful if you would report this to us. By doing so, you can save other readers from frustration and help us improve subsequent versions of this book. If you find any errata, please report them by visiting http://www.packtpub.com/submit-errata, selecting your book, clicking on the **errata submission form** link, and entering the details of your errata. Once your errata are verified, your submission will be accepted and the errata will be uploaded on our website, or added to any list of existing errata, under the Errata section of that title. Any existing errata can be viewed by selecting your title from http://www.packtpub.com/support.

## Piracy

Piracy of copyright material on the Internet is an ongoing problem across all media. At Packt, we take the protection of our copyright and licenses very seriously. If you come across any illegal copies of our works, in any form, on the Internet, please provide us with the location address or website name immediately so that we can pursue a remedy.

Please contact us at copyright@packtpub.com with a link to the suspected pirated material.

We appreciate your help in protecting our authors, and our ability to bring you valuable content.

## Questions

You can contact us at questions@packtpub.com if you are having a problem with any aspect of the book, and we will do our best to address it.

## 1 Getting Started with Enterprise PowerShell Scripting

The PowerShell language was developed to provide engineers with a method to quickly automate the provisioning and management of environments. In the last decade, PowerShell's use has greatly expanded, to encompass the setup and configuration of a myriad of hardware and software systems. Since third-party manufacturers are creating PowerShell modules to control their systems, PowerShell is becoming the automation language of choice. It is being used to configure multiple systems from multiple manufacturers, with a single set of PowerShell code.

Creating scripts for enterprise environments requires a robust scripting framework. A scripting framework consists of reusable code that you can leverage for a large majority of your scripts. This framework needs to be efficient, reliable, and secure. It must also provide logging capabilities, leverage answer files, and be flexible for execution on a wide variety of systems.

As you are exploring this book, you will learn a large number of tested and industry-proven scripting techniques. The examples you create will be applied at the end of the book in a Windows server scanning script. This will enable you to quickly integrate new sections of code into your own scripts, and will provide a trusted platform you can confidently deploy in your environment.

In this chapter, you will:

- Learn about the Windows server scanning script created throughout this book
- Explore performance optimizations implemented throughout each chapter
- Review different components you should include in all of your PowerShell scripts

### Windows server scanning script

There can be times in large enterprises where you need to determine the functionality of systems. If you don't have large asset management software, you will need the ability to perform a discovery. This book was designed to provide a significant jumpstart for the creation of a Windows server scanning script. The script takes into consideration the limitations of Windows components and provides extremely detailed information about systems.

Some of the items that you will be scanning for include:

- **Disk configuration:** You will be able to query the disk layout for a particular system and determine device type, drive letters, free space, and total disk size.
- **Scheduled task scanning:** You will discover the scheduled tasks that are not running as built-in accounts, providing visibility into non-standard configurations.
- **Windows processes:** You will identify the Windows processes on the system to determine what processes are running with alternate user credentials. This will help identify systems that have service accounts for process execution.
- **Windows services:** You will review the Windows services on the system to discover what services are not running as built-in accounts. This will further identify service account usage in your enterprise.
- **Installed software:** You will learn how to safely scan a system for software to identify software titles installed on a system.
- **User profiles:** You will evaluate all of the user profiles created on a system to determine the last login usernames and times. This will help identify teams that own the management of the servers.
- **Windows features:** You will determine the installed Windows features and roles on a system to help identify what role the server has in your enterprise.
- **Scanning files**: You will scan individual files on a system for strings. This provides the ability to identify items such as user credentials and configured server names.

This script is designed to provide a robust platform to scan your enterprise. Variations of this script have been used in multiple Fortune 500 companies and executed on well over 10,000 systems. The chapters in this book have been designed to enable you to quickly learn the core components to integrate this PowerShell tool into your personal repository.

### Performance considerations

This book dives into important performance considerations for your scripts. Since PowerShell is an extremely flexible language, you can complete the same activity in multiple ways. While both sets of code get the script to create the same results, there are ways to optimize your code to make it much more efficient. This becomes increasingly important as you evaluate large amounts of data.

Execution time for file scanning, for example, exponentially grows with the quantity of files and the file sizes. 100 files with 100 lines of code can quickly create 10,000 evaluations. Now, if you scale that to 6 million files in a storage volume times 100 lines of code, you are performing 600 million evaluations. Small code performance improvements can significantly optimize your scanning of scripts at scale.

Some of the performance considerations include:

- **Measuring commands**: You will evaluate a technique to measure the execution time for commands.
- **Cmdlet considerations**: You will learn which cmdlets are more efficient than others and how to avoid common cmdlet mistakes.
- **Regular expressions over arrays**: Regular expressions can be difficult to learn, but they provide significant performance improvements to your scripts. You will learn how to properly implement dynamic regular expressions in your scripts.
- **Switch statements**: You will evaluate how to perform data sorting with switches and why switch statements are significantly faster.
