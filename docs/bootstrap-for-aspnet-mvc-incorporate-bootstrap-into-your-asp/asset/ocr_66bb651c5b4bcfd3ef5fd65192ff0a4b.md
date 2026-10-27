The fluent implementation also provides IntelliSense inside Visual Studio, enabling developers to easily discover the Bootstrap configuration options. The library is no longer open source or free, although you can still view the original project on GitHub at `https://github.com/DmitryEfimenko/TwitterBootstrapMvc`.

The commercialized version of the library is available at `https://www.twitterbootstrapmvc.com/`.

# Including TwitterBootstrapMVC in
your project

You can include the `TwitterBootstrapMVC` library in your project in one of the two ways. The first and preferred option is to use the NuGet package, and the second option is to download and manually add a reference to the `.dll` file.

# Adding TwitterBootstrapMVC using NuGet

To include the `TwitterBootstrapMVC` library in your project using NuGet, perform the following steps:

+   1. In Visual Studio, open the **Package Manager Console** window, via the **Tools | Library Package Manager | Package Manager Console** menu.
+   
+   2. Inside the **Package Manager Console** window, type the following command and hit *Enter*:
+   
+      ```
+      Install-Package TwitterBootstrapMVC5
+      ```
+   
+   3. Next, navigate to `https://www.twitterbootstrapmvc.com/Download` and download a trial or purchase a personal or commercial license.
+   
+   4. Download the license file, it should be called `TwitterBootstrapMvcLicense.lic` and add it to the root of your project in Visual Studio.
+   
+   5. Open the `Web.config` file located inside the Views folder and add the following code to the `<namespaces>` element:
+   
+      ```xml
+      <add namespace="TwitterBootstrapMVC" />
+      <add namespace="TwitterBootstrap3" />
+      ```
+   
+   6. Finally, open the `Global.asax` file in the root of your project and add the following line of code to the `Application_Start` method:
+   
+      ```csharp
+      Bootstrap.Configure();
+      ```

```
Install-Package TwitterBootstrapMVC5
```

```
<add namespace="TwitterBootstrapMVC" />
<add namespace="TwitterBootstrap3" />
```

```
Bootstrap.Configure();
```