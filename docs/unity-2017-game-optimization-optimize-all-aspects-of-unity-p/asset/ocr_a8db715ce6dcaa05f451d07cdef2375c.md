Fortunately, it is not necessary to become absolute masters of the C# language to use it effectively. This chapter will boil these complex subjects down to a more digestible form and is split into the following subjects:

+   - The Mono platform:
+     - Native and Managed Memory Domains
+     - Garbage collection
+     - Memory Fragmentation
+   - IL2CPP
+   - How to profile memory issues
+   - Various memory-related performance enhancements:
+     - Minimizing garbage collection
+     - Using Value types and Reference types properly
+     - Using strings responsibly
+     - A multitude of potential enhancements related to the Unity Engine
+     - Object and Prefab Pooling

# # The Mono platform

Mono is a magical sauce, mixed into the Unity recipe, which gives it a lot of its cross-platform capability. Mono is an open source project that built its own platform of libraries based on the API, specifications, and tools from Microsoft's .NET Framework. Essentially, it is an open source recreation of the .NET Library, was accomplished with little-to-no access to the original source code, and is fully compatible with the original library from Microsoft.

The goal of the Mono project is to provide cross-platform development through a framework that allows code, written in a common programming language, to run against many different hardware platforms, including Linux, MacOS, Windows, ARM, PowerPC, and more. Mono even supports many different programming languages. Any language that can be compiled into .NET's **Common Intermediate Language (CIL)** is sufficient to integrate with the Mono platform. This includes C# itself, but also several other languages such as F#, Java, Visual Basic .NET, PythonNet, and IronPython. Of course, the only three exposed to us through Unity are C#, Boo, and UnityScript.