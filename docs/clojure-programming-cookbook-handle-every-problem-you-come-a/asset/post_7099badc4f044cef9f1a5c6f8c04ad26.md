
// Comment to get more information during initialization
//logLevel := Level.Debug
logLevel := Level.Info
//lazy val root =
project.in(file(".")).dependsOn(clojurePlugin)

//lazy val clojurePlugin = file("../../sbt-clojure")
addSbtPlugin("com.unhandlederexpression" % "sbt-clojure" % "0.1")

## Writing source code

We will create the Clojure source code under src/main/clojure:

```
(ns helloworld)
(gen-class
  :name cljbook.chapter05.HelloWorld
  :methods [#^{:static true} [sayHello [] void]])
(defn -sayHello []
  (println "Hello world from Clojure!"))
(gen-class
  :name cljbook.chapter05.Calculator
  :methods
  [#^{:static false} [add [long long] long]
   #^{:static false} [sub [long long] long]])
(defn -add [this a b]
  (+ a b))
(defn -sub [this a b]
  (- a b))
```

The preceding Clojure code has two classes:
cljbook.chapter05.HelloWorld and cljbook.chapter05.Calculator.
Then, we will look at Scala code. The following is the Scala code for calling classes defined in Clojure:

```
import cljbook.chapter05.HelloWorld
import cljbook.chapter05.Calculator
object Main {
  def main(args: Array[String]) = {
    println("I'm calling methods defined by Clojure from Scala.")
    HelloWorld.sayHello();
    val calc = new Calculator
    println("add: "+calc.add(1,2))
    println("sub: "+calc.sub(10,5))
  }
```

The following screenshot is the directory for this project:

![](img/d7b631352a782ee50160cf8eb995152f_324_0.png)

Then run the following:

```
$ sbt run
```

![](img/d7b631352a782ee50160cf8eb995152f_324_1.png)

## ClojureCLR

ClojureCLR is a way to use and port your Clojure code to a .NET runtime. There are different levels in achieving this integration; why you want to run your Clojure code on the CLR is going to be at the root of your usage and of its different patterns. You may simply have stumbled upon some fantastic looking Clojure code, but with all those brackets you are not sure if you can debug it or not yet, so you just want to embed the clj algorithm and be able to run this on the command line or through a Windows native interface. You may be tempted to create Windows User Interfaces from the usual Clojure REPL, which is also possible using ClojureCLR. You may also want to distribute you Clojure code into a Windows Dynamic-link Library (DLL) and give it to a customer so he can call it natively.

In this recipe, we will look at all those different options and will also learn how to be productive on the CLR.

While working through this recipe, you will also realize, again, how the memory footprint of .NET programs is often lower than their Java VM counterparts, which makes a nice argument for Clojure when running on memory-limited environments.

Writing this recipe made the author wanting to write more C# and other related .NET code and even to write some mobile apps. Now it's your turn to get excited.

## Getting ready

There is actually quite a lot we need to do to get ready for this recipe. Also, make sure to not go around the web and try to combine indications from other web sources with this one. Things have gotten a lot better over time with regard to installation and integration, so there is no need to revert to outdated instructions.

First, on Windows, it is recommended you install Visual Studio.

### Note

All examples in this recipe have been written using the Community Edition. Quite a few of the screenshots will be taken from there.

There is no particular plugin needed, but the examples in this chapter will make use of a few C# examples.

Along the way, we will also look at how to integrate the mono IDE from Xamarin Studio. Mono allows you to run .NET code on non-Windows machines; thus, you do not need to run the Windows-only Visual Studio and instead you can run your nicely integrated .NET and Clojure code everywhere.

## Installing Visual Code Studio and creating a project

This should be a no-brainer, apart from the required disk space, which takes up almost 10GB of your precious SSD drive:

### Visual Studio

Community 2015
with Update 1

Features   Languages

Select features

- Programming Languages
- Windows and Web Development
  - ClickOnce Publishing Tools
  - Microsoft SQL Server Data Tools
  - Microsoft Web Developer Tools
  - PowerShell Tools for Visual Studio [3rd Party]
  - Silverlight Development Kit
  - Universal Windows App Development Tools
  - Windows 8.1 and Windows Phone 8.0/8.1 Tools
  - Cross Platform Mobile Development
  - Common Tools

Select All

Reset Defaults

Setup requires up to 10 MB across all drives.

Back

UPDATE

Once the install is finished, you will create a new command line-based C# project, as shown in the following screenshot:

![](img/d7b631352a782ee50160cf8eb995152f_328_0.png)

Creating a new project will take you to a very familiar setup. To get ready to use our project with Clojure, we will use the NuGet package manager, which is the de facto standard in .NET for installing the Clojure libraries:

![](img/d7b631352a782ee50160cf8eb995152f_329_0.png)

Now we will install the Clojure libraries into the current project. In the NuGet console, type the following:

```
Install-Package Clojure -Version 1.7.0
```

Now NuGet will grab the basic stuff for you:

![](img/d7b631352a782ee50160cf8eb995152f_329_1.png)

A note here: ClojureCLR version 1.8.0 could not be successfully tested

before this book came out so that is why we stick to a slightly older version. For our purposes here, that will not be a problem.

You are now ready to execute your first bits of Clojure code from your C# project.

Let's stick in some usual interop code in the core of the main application:

```
using System;
using clojure.lang;
using System.Threading;
using clojure.clr.api;

namespace firstclojure
{
class Program
{
    {
        static void Main(string[] args)
        {
            IFn load = clojure.clr.api.Clojure.var("clojure.core",
"load-string");
            Console.WriteLine(Convert.ToString(load.invoke("(+ 1 1)")));
            Thread.Sleep(1000);
        }
    }
}
```

Executing the preceding, and assuming the installation went fine, the following will show up:

2

This means that the Clojure code has effectively been compiled using the load-string method, and has been eval-ed. We're sleeping a bit more in the end, so we have time to see the message. It is worth noting that the Objects explorer window shows a reference to the core Clojure libraries:

![](img/d7b631352a782ee50160cf8eb995152f_331_0.png)

Looking through the hierarchy, we can see the different methods that are available, and actually Visual Studio does a good job of showing you options while you type code.

This is very basic, and we will see how to do more interaction with Clojure later on.

For now, since the workflow is very similar, let's have a look at how to get ready on a platform-independent environment when using Mono Studio.

## Installing Mono Studio IDE

For those of us who do not have the chance to work on Windows, we can revert to installing Xamarin Studio. There will be some more gymnastic around, so if you are on Windows, probably sticking with the comfort of Visual Studio is a better option:

![](img/d7b631352a782ee50160cf8eb995152f_332_0.png)

# Welcome to the Xamarin Studio 5.10.1 Setup Wizard

The Setup Wizard allows you to change the way Xamarin Studio 5.10.1 features are installed on your computer or to remove it from your computer. Click Next to continue or Cancel to exit the Setup Wizard.

Once the setup is done, the new project flow should feel similar; you can create a new command line-based project:

![](img/d7b631352a782ee50160cf8eb995152f_332_1.png)

# Choose a template for your new project

- Cross-platform
  - App
  - Library
  - Tests
- Other
  - .NET
  - ASP.NET
  - Miscellaneous

- Console Project
- Empty Project
- Gtk# 2.0 Project
- Library
- NUnit Library Project
- F# Tutorial

Console Project
Creates a new C# console project.
