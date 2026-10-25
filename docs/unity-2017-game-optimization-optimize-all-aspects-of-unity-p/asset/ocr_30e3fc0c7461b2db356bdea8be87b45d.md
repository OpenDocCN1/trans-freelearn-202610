# # WebGL optimizations

Unity Technologies have also released a number of blog posts covering WebGL
applications, which includes some crucial information about memory management that all
WebGL developers should know. These can be found at the following links:

+   • https://blogs.unity3d.com/2016/09/20/understanding-memory-in-unity-
+   webgl/
+   • https://blogs.unity3d.com/2016/12/05/unity-webgl-memory-the-unity-
+   heap/

# # The future of Unity, Mono, and IL2CPP

When the first edition of this book was written, IL2CPP was being teased in a variety of
blog posts, and Unity was still running a very old version of Mono. Well, the good news is
that IL2CPP has finally landed, but the bad news is Unity is still running a very old version
of Mono. This is an unfortunate consequence of how various elements of the Mono
Framework are licensed, meaning that Unity Technologies has only been able to update
Mono on an infrequent basis.

After many years, Unity is still only able to make use of the .NET 3.5 library
functionality, which is about a decade old at this point. This limits what kinds of .NET
library classes we can use, restricts certain C# language features, and limits performance
since many enhancements would have been made to the library during this time. However,
to the great relief of many Unity developers, an experimental upgrade to a more recent
version of Mono has been in beta for a while and is now available in Unity 2017 by
switching the Edit | Project Settings | Player | Configuration | Scripting Runtime
Version option to Experimental (.NET 4.6 Equivalent). This setting upgrades the Mono
runtime and allows us to make use of the .NET 4.6 functionality, which is only a couple of
years old at the time of writing.

This feature is still experimental, however. This could lead to obscure bugs and crashes
until it's been deemed stable. At which point, the Mono upgrade will become permanent so
that all users can enjoy it. For the time-being, we should only use this option if we need it
for a vital bug fix or feature that we cannot work around. Also, there is never a guarantee
that the version fixing your bug isn't going to introduce an even worse bug. However, at the
very least, hope is on the horizon that Unity is serious about making the upgrade happen
sooner rather than later, as there are regular bug fixes around this area each update.