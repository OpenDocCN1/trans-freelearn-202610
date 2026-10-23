Computer programs written in a compiled code programming language (C, C++, Delphi, and so on) go through a **compiling process**. This process takes the easily readable source code and compiles it into a form that the targeted processor can understand and execute, assembly code. Assembly code will be further optimized for the target platform by the assembler into machine code, at which point only computers can make sense out of it. Now taking that machine code and turning it back into source code is a difficult process and only specialized tools can do a partial job. The nuances of the original code, such as comments, variable names, and the like, are lost in translation and would have to be rebuilt somehow. Taking the machine code and turning it into assembly code is a much easier job and can be achieved with a disassembler. There are many types of disassembler, implementing various techniques to recover the machine code, and a discussion about the various types and techniques is outside the scope of this book and would probably bore you to death.

To get our feet wet with looking at recovered machine code, we will be discussing the IDA Pro disassembler. You can download a copy of the IDA Pro freeware version from `https://www.hex-rays.com/products/ida/support/download_freeware/`. This version is missing many of the bells and whistles of the full-fledged production but there is still enough for us to get going.

For more information and tutorials on how to use Ida Pro, head on over to `https://resources.infosecinstitute.com/topic/basics-of-ida-pro-2/.`

We are going to install the free edition of IDA Pro on the REMnux VM. Follow along with the instructions to get IDA Pro installed and running:

+   1. Download the Linux version to the VM from the Hex-Rays website and install it by running the following commands:

```
```bash
remnux@remnux:~/workdir$ cd ~/Downloads/
remnux@remnux:~/Downloads$ ls
idafree70_linux.run
remnux@remnux:~/Downloads$ chmod +x idafree70_linux.run
remnux@remnux:~/Downloads$ sudo ./idafree70_linux.run
```
```