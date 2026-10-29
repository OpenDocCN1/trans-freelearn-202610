The rest of the code is rather easy to go along, and you will notice, of course, the call to the get-sunset function that you wrote earlier on. On running all this through the REPL or through the command lein clr run -m hello.simpleui2, you will now be able to get the sunset times for all those beautiful Japanese cities:

![](bbox=[0.06, 0.186, 0.595, 0.467])

# How it works...

Now that you have seen through an extensive list of CLR interops, it is time to see how all this works when calling Clojure code from .NET. Things get a little bit more complicated, so you will have to stay focused. First of all, you want to be able to generate executables and Dynamic Libraries from the Clojure code you are writing. Once this is all going smoothly, we will switch to importing those generated objects into a Visual Studio project, and for the sake of completeness, a Xamarin Mono-based project as well.

# Compiling Clojure code for .NET

Back to the hello.net namespace that you have created earlier on, by adding the usual interop glue; we can add a main method that will be the main entry point when calling the executable: