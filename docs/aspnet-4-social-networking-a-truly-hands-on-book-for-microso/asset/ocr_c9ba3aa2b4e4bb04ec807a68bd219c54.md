This Person table will be used later in our .NET solution via the ADO.NET Entity Framework (.edmx file).

# Building Extensibility using Managed Extensibility Framework (MEF)

To support extensible architecture we need to build several extensibility points that if needed can be easily replaced seamlessly with different implementations. For example, replace one caching implementation with another caching one.

However, this should be achievable with the least overall impact i.e. it should not require code change and recompilation of the application. The replacement should ideally be as trivial as replacing the existing assembly with the new implementation.

There are a few frameworks that help achieve this including Unity framework, Structure Map, Castle Windsor, etc. However, we have decided to use Managed Extensibility Framework (MEF) which is an integral part of .NET 4. Visual Studio 2010 itself uses MEF internally to provide extensibility.

We chose MEF over others because of the simplicity it has to offer including aspects like a lot less code to code to achieve the necessary functionality, simple API support with attribute style programming, and no need to maintain any configuration files.

Following are three core concepts associated with MEF:

+   - **Export(s)** is/are potential candidates in the applications that can be replaced at runtime. It exposes "Contracts" and Contract can be anything like .NET Interface or Class or property.
+   
+   [Export(typeof(ICache))]
+   public class Caching: ICache
+   {
+       public object Get(string cache_key)
+       {
+           //Specific caching implementation goes here
+       }
+   }

+   - **Imports** are used to consume exported instances. Consuming application will be as:
+   
+   [Import()]
+   Private ICache cache;