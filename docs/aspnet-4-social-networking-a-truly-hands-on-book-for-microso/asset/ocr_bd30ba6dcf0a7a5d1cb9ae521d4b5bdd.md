The view in the ASP.NET is built by MVC using standard HTML (done by using HTML helper methods). There are no ASP.NET server controls available and hence the developers end up writing most of the HTML themselves, but in turn this provides them complete control over the HTML and thus handle the accessibility requirements in a much better way. While the support in Visual Studio at this time is limited in terms of HTML controls for ASP.NET MVC, you need not write everything on your own as you get the option to use different view engines. There are third party controls available today that can be used with ASP.NET. You can pick from NVelocity, Brail, NHaml, Spark, etc. view engines to build your views.

> For more details on ASP.NET MVC you can refer to the MSDN library here: http://msdn.microsoft.com/en-us/library/dd381412.aspx.

Now let's address the final question on selecting between MVP (essentially using ASP.NET Web Forms approach) and MVC.

# Choosing between MVP and MVC

Both MVP and MVC patterns are capable of generating enterprise class applications. While MVC is definitely a much better pattern, it is also true that as of today the productivity benefits that you get with the MVP approach is far superior. MVC pattern adds a significantly steep learning curve for the current web developers or even the new ones who are just starting.

Given the expectation of rich user interface for a social networking site, using MVC also additionally means lot more work towards creating custom controls for almost every display requirement. MVP allows us to work with the large number of ASP.NET server controls.

When starting on a new application you can argue more convincingly in favor of MVC. However in our case, since this is an update of the book and the associated source code is building on top of the previous edition, using MVC would have meant a significantly long cycle.

There are no migration wizards available today that help in easily taking some MVP based code and converting it to MVC. Had we selected MVC, it would have meant a rewrite of the presentation layer and would have impacted the release timelines of this book.