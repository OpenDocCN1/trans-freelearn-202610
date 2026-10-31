
# Known Issues

* When compiling the GUI console, some antivirus apps (such as Avast or Avira) report a Win32:Malware-gen or the like. This is because their heuristic search mechanisms signal a false positive for the binaries compiled by Red. The solution for this is to add the `C:\\ProgramData\\Red` folder and other folders where you compile Red executables to the Excluded Folders list of your antivirus software interface, so that they are whitelisted and not checked by the antivirus program.

* When Red is started in a console from a folder other than the installation folder, you can see the following error—`PROGRAM ERROR: Invalid encapsulated data`. This is a known issue ([https://github.com/red/red/issues/543](https://github.com/red/red/issues/543)) caused by the REBOL support at this moment. Currently, running Red requires a wrapping shell script `red.bat`:
  * Rename `red.exe` to `red-exe.exe`
  * In the same folder, create a `red.bat` file with this contents: `@"%~dp0red-exe.exe" %\*`
  * Now, giving the `red` command will automatically start this `red.bat` command file

# Installing Red on Linux and macOS

To install Red on a Linux or macOS system, you would need to follow these steps:

1. Save the file from [http://www.red-lang.org/p/download.html](http://www.red-lang.org/p/download.html) in a folder of your choice, say `/home/user/red` (where `user` is your username).
2. Rename the file `red-num` to `red`.
3. In your Terminal, go to the `red` subfolder, and do a `chmod u+x red` to give the user executable rights on the Red binary.
4. Install some 32-bit supporting libraries if you work on a 64-bit Linux system (this is needed for executing REBOL 2, which needs these libraries.) Open a Terminal app and give the following commands:
```
$ sudo dpkg --add-architecture i386
$ sudo apt-get update
$ sudo apt-get install libc6:i386 libcurl3:i386
```
5. Now simply run Red using the `red` command in a Terminal window.

The Red console will be built, but only the first time. The console executable is built inside the `/home/user/.red` folder and is named in the format `console-yyyy-m-dd-nnnnn`.

Afterwards, the command-line console opens:

![](assets/64983179-6fdb-47e4-a414-9ce648561387.png)

To make Red available everywhere in the filesystem, add the following line to your `.profile` and `.bashrc` scripts:
```
export PATH=$PATH:/home/user/red
```
(In the preceding command, `user` is your username.)

Then, open a new Terminal app. Now the Red executable can be started from any directory.

If you have a Red Linux utility script in `/bin` like I did, you have to rename it as follows:
`cd /bin`
`sudo mv red red_old`

If you encounter an Invalid compressed data problem, this is the same REBOL issue as mentioned with Windows. It can be remedied by creating a shell script, `red.sh`, with the following contents:
```
#!/bin/bash
red $@
```
Don't forget to make the shell script executable with `chmod u+x red.sh`.

An alternative to the Red command-line version in Linux is to install WineHQ ([https://www.winehq.org/](https://www.winehq.org/)), a tool that makes it possible to run Windows programs on Linux. Then you can run the Red Windows version with the graphical console.

For Arch Linux, FreeBSD, and other specific distributions, you can find further instructions on the download page.

Red can also be installed by using the Chocolatey package manager ([https://chocolatey.org/packages/red](https://chocolatey.org/packages/red)).

Regardless of which OS you are on, to quickly view the version number of your Red installation, open a Terminal and type:
```
red -V
```
This will give you the version `n.u.m` (for example, `0.6.3`).

# A look at the source code

Red's source code can be found here: [https://github.com/red/red](https://github.com/red/red). If you're curious, you can download an archive file locally (click the Clone or download button) and unzip it for a closer look, for example, inside your Red installation directory. Alternatively, with `git` installed on your machine, you can clone the source code with the following command:
```
git clone https://github.com/red/red.git
```
For easy reference, rename the source folder as `red-source`. Here is an overview:

![](assets/fa24ee2f-cdd4-4843-9d1a-5bcda1e2979b.png)

You will notice that lots of source files have a `.r` extension, which means these are REBOL files. Indeed, at this stage, the Red framework is scaffolded with REBOL. Some important files are:
* `red.r`: This is the Red and Red/System compiler
* `run-all.r`: This is the starting point for running the complete Red test suite

The goal of the Red team is to completely rewrite this scaffolding in Red and so remove the dependency on REBOL. This will probably happen in v2. Then, Red will be completely self-hosted and every new version will bootstrap from an older Red version.

At the same level as this folder, you could create a `projects` folder, in which you can start developing:
* `red`
  * `red-source`
  * `projects`
  * `red` binary or shell script

# Working interactively in the Red console

Like any dynamic language, Red has a REPL console. The `red` command without arguments (open or double-click `red.bat`) starts a graphical window console on Windows and macOS, and a command-line console on Linux. If you need a command-line console on Windows and macOS, just use the command `red --cli`.

The console is a full Red interpreter and expects the input of Red code after the `\>>` prompt, to evaluate and then execute it when pressing `Enter`. It's a live interaction with the language. Simply giving it a value (here a string indicated by `""`) returns that value after the `\==`:
```
;-- see Chapter02/console-examples.red:
>> "Red is awesome"
== "Red is awesome"
```
(You can find the source code of these examples in `ch2/console-examples.red`. In all code files, we'll indicate the returned output with a `;==` prefix, like this, `;== "Red is awesome"`).

What happens if you type `Red is awesome` in the console? The REPL throws an error at you—`Script Error: is does not allow word! for its 'field argument`. Red doesn't see that the input is a string, so doesn't understand it.

Go ahead and do some calculations:

![](assets/6f0e1e2a-2711-4e8b-8cb7-367b9b9eed56.png)

In the second line, we see our first syntax error. It means that you have to use a space before and after a binary operator such as `\*`. Notice that `/` using integers returns an integer result.

The console has a built-in help and documentation system that you can start by entering `?` or `help`:

![](assets/4d5f38a2-cb1c-414b-bf0f-b169930c43ef.png)

Try out the examples to get a feel for its search and completion features (the word **molded** means: in a readable format).

Here are some other useful hints for working in the console:
* The up arrow and down arrow are used to retrieve the previous and next console commands, which can then be altered and/or re-executed. In fact, a history of the console commands is stored in the `system/console/history` block, and from version 0.6.4 this is preserved between sessions. In [Chapter 6](#0f4412a2-a1fb-4241-a1cf-98fed354b645.xhtml), *Working with Files,* in the *Loading and saving files* section, we'll learn a nice trick to save a history of console commands to a file for easier processing.
* To clear the screen, use `CTRL+L`.
* You can close the console at any time with the close window icon, or from the prompt by entering `q` or `quit` + `Enter` (on Linux, also with `CTRL+D`).
* `Home` and `End` position the cursor at the start and end of a line.
* Use the `Esc` key when you want to get out of a (running) statement to the prompt in the GUI console; for the command-line interface use `Ctrl+C`.
* Copy/paste code (even entire Red scripts) with the mouse or the usual keyboard shortcuts (`Ctrl+C` - `Ctrl+V`).
