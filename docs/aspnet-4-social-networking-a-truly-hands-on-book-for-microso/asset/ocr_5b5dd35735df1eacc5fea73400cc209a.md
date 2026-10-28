# Design

Let's take a look at the design for this feature.

# Ratings

With the ASP.NET AJAX Control Toolkit you are able to create horizontal or vertically positioned stars; stars that can be run from left-to-right, and right-to-left. You can even swap out the stars for any other image such as happy faces, if you really want to.

![](bbox=[0.228, 0.332, 0.299, 0.369])

> More information regarding the toolkit can be found here:
> **http://www.codeplex.com/AjaxControlToolkit** and
> **http://www.asp.net/ajax/ajaxcontroltoolkit/**.

The one thing that the control in the toolkit does not do, is allow you to show multiple attributes to be rated for an item. It also does not allow you to aggregate those ratings to be rolled up into one global rating. It is this more flexible control that we will discuss and build here.

Before we discuss rating attributes of an item, we need to discuss what an item is. In our community site, we may want to rate just about anything that is awfully vague and generally hard to work with. So we will say that our definition of "anything" is any table in the database (you will see more use of this object concept when we discuss comments and tags). In order to keep track of our objects, we will need to create a container which we can use as a hard reference.

For this, we will create a SystemObjects table. This will hold the name and SystemObjectID for each of the tables that we want to add ratings to. If we add more tables to our system that could benefit from ratings, we can easily extend our rating tool to work with the next object.

Now that we know what our objects are, we can discuss our attributes that will actually get individual ratings. For this, we will need the ability to have a flexible system that allows each unique object to have its own attribute definitions. We will create a SystemObjectRatingOptions table to manage these attributes. Here you will see the name of the attribute and its description. We will show the name in our display and add the description to a tooltip. In addition to this, we will create the relationship to our SystemObjects table. This will allow us to have custom attributes for something like images—as many as we like.