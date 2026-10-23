The GCD, and thus the Domain, contains:

+   - Simple properties of the Domain object itself
+   - Domain-level objects
+   - Configuration objects for more complex aspects of the Domain environment
+   - Pointers to other components, both as part of the CE environment and external to it

It's a little bit subjective as to which things are objects and which are pointers to other components. It's also a little bit subjective as to what a configuration object is for something and what a set of properties is of that something. Let's not dwell on those philosophical subtleties. Let's instead look at a more specific list:

+   - **Properties**: These simple properties control the behavior of or describe characteristics of the Domain itself.
+     - **Name and ID**: Like most P8 objects, a Domain has both a Name and an ID. It's counterintuitive, but you will rarely need to know these, and you might even sometimes forget the name of your own Domain. The reason is that you will always be connecting to some particular CE server, and that CE server is a member of exactly one Domain. Therefore, all of the APIs related to a Domain object are able to use a defaulting mechanism that means "the current Domain".
+     - **Database schemas**: There are properties containing the database schemas for an Object Store for each type of database supported by P8. CM uses this schema, which is an actual script of SQL statements, by default when first fleshing out a new Object Store to create tables and columns. Interestingly, you can customize the schema when you perform the Object Store creation task (either via FEM or via the API), but you should not do so on a whim.
+     - **Permissions**: The Domain object itself is subject to access controls, and so it has a Permissions property. The actual set of access rights available is specific to Domain operations, but it is conceptually similar to access control on other objects.
+   - **Domain-level objects**: A few types of objects are contained directly within the Domain itself. We'll talk about configuration objects in a minute, but there are a couple of non-configuration objects in the Domain.
+     - **AddOns**: An AddOn is a bundle of metadata representing the needs of a discrete piece of functionality that is not built into the CE server. Some are provided with the product, and others are provided by third parties. An AddOn must first be created, and it is then available in the GCD for possible installation in one or more Object Stores.