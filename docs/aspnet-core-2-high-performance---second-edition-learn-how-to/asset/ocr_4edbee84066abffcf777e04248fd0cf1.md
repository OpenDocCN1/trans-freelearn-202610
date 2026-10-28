There are many tools that you can use to help with centralizing your debug information. You can retrofit some of them. However, to get the most out of them, you should decide what to measure upfront and build telemetry capabilities into your software from the start.

# Logging

Logging is vital in a high-performance application, so while it is true that logging adds some overhead and can slow down execution, omitting it would be short-sighted and a false economy. Without logging, you won't know what is slow and requires improvement. You will also have other concerns, such as reliability, for which logging is essential.

# Error logging

You may be familiar with the excellent ASP.NET package called **Error Logging Modules and Handlers (ELMAH)**, used to catch unhandled exceptions (http://elmah.github.io/). ELMAH is great for existing applications, as you can drop it into a live-running web app. However, it's preferable to have error logging built into your software from the start.

Unfortunately, ELMAH does not yet support ASP.NET Core, but there is a similar package called **Error Logging Middleware (ELM)**. Adding this to your web application is just as simple as installing Glimpse, but it doesn't have all the features of ELMAH. ELM isn't really suitable for production use, but it can still be useful locally to aid debugging.

First, add the `Microsoft.AspNetCore.Diagnostics.Elm` NuGet package to your project. You can use the graphical tooling of your IDE to do this (the methods will vary depending on what IDE you are using, for example Visual Studio or VS Mac). Alternatively, you can use the following command in the Terminal, which works on all the supported platforms:

```
dotnet add package Microsoft.AspNetCore.Diagnostics.Elm
```

This will add a line to the `.csproj` file, which should look something like the following:

```
<PackageReference Include="Microsoft.AspNetCore.Diagnostics.Elm" Version="0.2.2" />
```

The way packages are referenced in .NET Core 2.0 is different from .NET Core 1.0. They are no longer specified in a JSON file and although you can edit the XML project file manually, it is easier to use the supplied tooling. You can use the `dotnet migrate` command to upgrade an old `project.json`-based solution to an `msbuild`-based `.csproj` one.