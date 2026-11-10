# 23
Using DFC

In almost every project that you will execute based on Documentum, you may need to customize and extend basic content management functionalities provided by the Content Server.

You might be required to write some custom lifecycle or workflow methods or perform some business-specific customizations on existing Documentum products such as Desktop or Web Publisher. Would it not be difficult if you had to first understand and learn Documentum native server API calls and a proprietary language such as Docbasic to achieve these goals?

Well, Documentum provides you with a much simpler wrapper interface that shields you from making native calls directly and reduces the learning curve involved in getting familiar with any proprietary language and its syntax. The answer is DFC.

DFC is short for Documentum Foundation Classes and is provided as a set of Java interfaces and their implementing classes. However, for those who are working on a Microsoft COM (Component Object Model) environment, DFC comes with a Documentum Java-COM bridge (DJCB) for accessing Java interfaces. Lastly, in a .NET environment, you can utilize DFC PIA (Primary Interop Assembly) to access DFC interfaces. Furthermore, DFC includes a Business Object Framework (BOF) enabling you to wrap your business rules in the form of reusable components, allowing you to extend the functionality provided by DFC implementation classes.

In a nutshell, DFC exists as a Framework exposing the Content Server's content management functionality for you to effectively harness.

> DFC exposes some server API methods such as apiGet and apiSet to access Content Server functionality by making native server API calls. This has been provided only for backward compatibility with older models that were not based on DFC. You should avoid using such server API methods and instead use alternative methods in DFC that can achieve the same purpose.