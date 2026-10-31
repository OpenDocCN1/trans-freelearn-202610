
* Use `pwd` or `what-dir` to get the current folder (the folder the REPL was started in). The output will be as follows: `%/E/Red/`. `%` indicates a file path, its parts are separated by `/`.
* To change the current folder, use `cd`. For example, `cd %/E/Red/apps/google-finance`.
* Multiple Red consoles can be open at the same time.

The folder from which Red is started up is given by typing `system/options/path`.

Changing the prompt to show the time is easy and gives a glimpse of Red's power. See the following command:

```
system/console/prompt: rejoin [now/time " >> "]
;== "15:02:19 >> "
```

You can find the source code of the REPL in `red-source/environment/console`.

# Running and compiling your first script

Experimenting in the REPL is fine for small code fragments and you should definitely use it, but soon you will want to keep your code in a file. Use your preferred code editor (or look at the last Development environments for writing Red section of this chapter for editors and IDEs with support for Red development) and type in the following text:

```
;-- see Chapter02/hello-world.red:
Red [
    Title: "Simple hello world script"
]
print "Hello Red World!"
```

Save the file as `hello-world.red`; `.red` is the extension for Red source code files.

The `Red \[ \]` header block is mandatory; we'll see its purpose in the next chapter.

## Executing a script in the Red console

Using the Red console, we can execute this script in five ways:

1. If it is a small script, you could just copy the code that starts after the header and paste it into the REPL.
2. Right-click on `hello-world.red` and choose Open With. Browse to the Red `bat` file (on Windows), and select it. The console opens up with the output message. From now on, double-clicking on a Red source file will execute it in the Red console.
3. Open a Terminal (in Windows with `cmd` or a PowerShell console; in Linux/macOS by right-clicking on a folder and choosing Open in Terminal) and navigate to the folder where the source file is. Then type in the command-line, `red hello-world.red`.
4. At the Red console prompt `\>>`, type `do %hello-world.red` to obtain the same result. The `do` is a function that evaluates its argument, in this case a source file. This is the way to execute a script from within the console.

You will get the following Access error if the script file cannot be found, such as `red>> do %does-not-exist.red`:

`*** Access error: cannot open: %does-not-exist.red`

To avoid this error if the file `script.red` exists, but it sits in a different folder `tests`, if `tests` is a subfolder—`do %tests/script.red`, if `tests` is a folder one level up—`do %../tests/script.red`.

But you can also move the current active folder of the Red REPL (initially the folder where it was started) to where the file resides, by doing `cd tests` or `cd ../tests`, and then `do %script.red`

5. From the menu, open File | Run. A file explorer window opens where you can search for the source file. Opening it runs the code with the `do` command.

In all these cases the code is interpreted. This also means the script cannot contain Red/System code. In case you wondered, this is not JIT Just-In-Time compilation to native code; this is planned for a later version of Red.

If you have downloaded the Red source code as indicated in the previous section, open a Red console in that folder and run some of the demo scripts to see Red in action. For example, a universal Hello world:

```
red>> do %tests/hello.red
```

You should see the following output:

![](assets/81c65737-f30d-452a-ac43-9088dc5fe1e1.png)

If you take a look at the source code, you see that it just prints these strings. This shows that Unicode is no problem for Red!

## Compiling a script to an executable

Now we examine two different modes for compiling a Red program.

### Development mode

To compile a Red program to native code, we have to do the following:

1. Open a Terminal window and go to the folder where your script lives.
2. Give the command `red -c hello-world.red`.

Here is the output (on Windows in PowerShell):

![](assets/8e8a2590-a5a0-4f77-bb78-ce102760e1d9.png)

When a `-c` compilation is done, Red looks in this folder for a shared library `libRedRT` (`.dll` in Windows or `.so` in Linux/macOS); if this doesn't exist, it is compiled. The `libRedRT` shared dynamic library contains the whole compiled standard library and RunTime in precompiled form, and its size is less than 1 MB. As you can see, this compilation takes about a minute. But this only occurs the first time and is done for caching reasons. The next time, a compilation of any program in this folder is very fast:

![](assets/58438bfd-8ce6-4243-910e-edaa484347ab.png)

This is compiling in Development mode, which is the default. When working on the same code, subsequent compilations are much faster. The result in this case is a native binary `hello-world.exe`; so run it as `./hello-world.exe` to show the hello message. It needs the `libRedRT` library in the same folder to run!

![](assets/a26f0b56-8460-4dd6-99ec-0c4578add5f1.png)

The code in `libRedRT` won't change unless you update your Red version. Upgrading the Red binary to a newer version will automatically upgrade `libRedRT` on first invocation. If needed, you can run a `red clear` command in your compilation folder to remove all files from a previous version. If your script contains Red/System code, use `red -u` or `red --update-libRedRT` to rebuild `libRedRT` and compile the script.

### Release mode

When development and testing are finished, and you are ready to deploy to production, you'll want to compile your code to one standalone executable that includes `libRedRT` and all other needed dependencies. This is done with the `red -r hello-world.red` command, forcing release mode with the `-r` flag. Here you see screen output from a compilation on Windows:

![](assets/4690e0f9-6c60-4070-ab67-3f1f40f165b3.png)

The compilation time is a lot longer in this mode, but you'll need to do it only once. The resulting binary size is somewhat increased (it contains the runtime and makes additional runtime functions available), but still quite small. To deploy, you only have to copy the resulting binary to your production environment.

This compile mode is also required when the program contains `#include` statements, or when it contains Red/System code.

The Red consoles can be found in the source code `/environment/console` folder, respectively in the CLI and GUI subfolders. The consoles are included in the Red toolchain, but with what we now know, we can compile and then use them separately like this:

```
red -r console.red
```

Here is the command to compile the GUI console:

```
red -r -t Windows gui-console.red
```

To include any graphical stuff, you have to add the `-t` platform flag, as `-t Windows` in the preceding command.

### Other compilation options

To see the documentation on the `red` command, type `red –h` in a Terminal. Here are some useful options:

* Use `–o` to name the output binary differently.
* Use `–d` in debugging mode. Functions used to print debug info (such as `print` and `probe`) show their output in the Red console.
* Use `–dlib` to make a shared library from code, for example, for a code file containing only functions.
* Use `-e` in combination with either `-c` or `-r` to compile in encapped mode; the runtime will be compiled and included in a single binary, together with your script. However, your script is not compiled; it is converted to a special format (Redbin) and compressed. The result is a standalone executable, and your script will be run by the interpreter. This can be useful when you encounter a compiler limitation, but your code can still be interpreted.
