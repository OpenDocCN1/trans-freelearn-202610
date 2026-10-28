
## An Enterprise Approach to our Community Framework

We would need to create the composition logic for composing every part we deal with. For example Log, Cache, Email, Configuration, Redirector, etc. so instead of repeating the composition logic all over the code, we will create a static class to have centralized logic for composition.

```csharp
//Fisharoo/Common/MEFManager.cs
using System;
using System.ComponentModel.Composition;
using System.ComponentModel.Composition.Hosting;
using System.IO;

namespace Fisharoo.Common
{
    public static class MEFManager
    {
        public static void Compose(object o)
        {
            var container = new CompositionContainer(
                new DirectoryCatalog(Path.Combine(
                AppDomain.CurrentDomain.BaseDirectory, "bin")));

            var batch = new CompositionBatch();
            batch.AddPart(o);
            container.Compose(batch);
        }
    }
}
```

To read the assembly from directory, **DirectoryCatalog()** method is used. Once the assembly providing the implementation for **IRedirector** is found, it is picked up by a program and composed in application using **container.ComposeParts()**.

We invoke this composition logic from **Application_Start()** method.

```csharp
void Application_Start(object sender, EventArgs e)
{
    // Code that runs on application startup
    MEFManager.Compose(this);
    log.EnsureInitialized();
}
```

### Data Access layer

To implement the domain model as discussed in domain driven design section above, we will use the Entity Framework to create the data access layer. For this we need to add a new project to our solution. We will call it DataAccess.

In this project, we will create two classes and one edmx file. The first class will be our connection object (named Connection.cs), and the second file will be called PersonRepository.cs. Once you have those class files created, add a new ADO.NET Entity Data Model by right-clicking the DataAccess project and select Add New Item. From the Data tab select ADO.NET Entity Data model as shown in the following screenshot:

![](img/15a62e134135dde036b0ac3cbb01dab6_74_0.png)

Let's call it Fisharoo.edmx. After this the wizard will drive you through various steps as explained below. Since we have already created a table in the database and want our model to be based on that table, we will select **Generate from database** in the first screen and click **Next**. Select appropriate connection string settings and click **Next**.

In the next screen choose your Database objects from **Tables | Person** as follows and provide Model Namespace as **FisharooModel** and click **Finish**.

This takes care of the base framework for some demonstrations. Now, let's dig into those two class files that we have created — **Connection.cs** file and the **PersonRepository.cs** file. The **Connection** class gives us a way to encapsulate our **DataContext** creation. It has one method that returns the **FisharooDataContext** object. If we have more data contexts to work with down the road, they could go here too and we can rename the current **GetContext()** method to a more meaningful name.

```csharp
//Fisharoo/DataAccess/Connection.cs
namespace Fisharoo.DataAccess
{
    public class Connection
    {
        public static FisharooDataContext GetContext()
        {
            FisharooDataContext fdc = new FisharooDataContext(ConfigurationManager.ConnectionStrings["FisharooConnectionString"].ToString());
            return fdc;
        }
    }
}
```

Note that we are spinning up a new instance of our **FisharooDataContext** class with a reference to our connection string that we had put in the **web.config** file earlier.

Now, let's move over to the **PersonRepository** class. The Person table that we created earlier is only for demonstration purposes, and so is the repository. Here is that class.

```csharp
//Fisharoo/DataAccess/PersonRepository.cs
using System.Collections.Generic;
using System.ComponentModel.Composition;
using System.Linq;
using Fisharoo.Interfaces;

namespace Fisharoo.DataAccess
{
    [Export(typeof(IPersonRepository))]
    public class PersonRepository : IPersonRepository
    {
        public List<string> GetAllNames()
        {
            List<string> names = new List<string>();
            FisharooDataContext dc = Connection.GetContext();

            var people = from p in dc.People
                         select p;

            foreach (Person p in people)
            {
                names.Add(p.FirstName + " " + p.LastName);
            }

            return names;
        }
    }
}
```

Note that it has an Export attribute so that we can swap the implementation out later if we need to. As with all MEF classes (and for good design reasons) we have also inherited from an IPersonRepository interface, which clearly defines this class.

There is only one method in our repository now. It is responsible for simply selecting all the people in the Person table and returning a list of the first and the last names of those people. We have a list of type strings for the names collection, which will be returned at the end of the method. We then fetch our DataContext for our query. Once we have the DataContext defined, we can perform our query.

We declare a local variable with the var keyword. This allows us to declare a variable with a dynamic type. In this case, we will be selecting a collection of our Person classes. But we don’t really know that for sure! Nor do we really need to know that.

The query we see in the code is to select all the people from the Person table and return a collection of those people. It is very much like SQL, however in reverse order, i.e. from comes first and select comes last. This is how LINQ queries are written and it is due to this ordering that Visual Studio Intellisense works with.

Once the query is executed the results are fetched into the people variable which is of type ObjectSet<Person>. This helps us to iterate over it in a loop. When we created edmx, Entity Framework automatically creates People ObjectSet as part of FisharooDataContext class.

Finally in the code we have a loop, where we concatenate the first name and last name of each person and add them to our names collection. Once this is completed, we simply return that collection back to the user of this method.

The demonstration given here is very simple, as we didn't want to overwhelm you. You would get the idea across as to how the Repositories will work in this application. We have also touched upon the basics of LINQ.

At this time, the solution structure is looking like the following screenshot:

![](img/15a62e134135dde036b0ac3cbb01dab6_78_0.png)

### Summary

In this chapter, we have covered creating an enterprise framework to handle the essential needs of most web applications. We have discussed architecture layering, domain driven design, related patterns, and best practices. We also worked through some code to create Components for the basic framework classes that we will be using in our application. We discussed how we will approach our data access using some new technologies and existing patterns. We also looked at how we will use MVP pattern in the presentation layer. This chapter should provide you with a solid foundation on top of which we can now begin to build our application.

## User Accounts

Without people, your community doesn't exist!

For any community site to be considered successful, it must first have a group of dedicated users. While there are other measures, however in most cases the larger the community's population the more successful it is considered to be. It would then make sense that we create a way for users to come to our site, create an account, and become a part of our community. The following screenshot shows how the registration page will look like:

![](img/15a62e134135dde036b0ac3cbb01dab6_79_0.png)
