# Introducing the Domain layer pattern

The following is Martin Fowler's definition of the Domain layer (http://martinfowler.com/eeaCatalog/domainModel.html):

> "An object model of the domain that incorporates both behavior and data".

Like the Service layer, this pattern adds a further layer of Separation of Concerns and factoring of the application code, which helps manage and scale a code base as it grows.

> " At its worst business logic can be very complex. Rules and logic describe many different cases and slants of behavior, and it's this complexity that objects were designed to work with. A Domain Model creates a web of interconnected objects, where each object represents some meaningful individual, whether as large as a corporation or as small as a single line on an order form."

Martin's reference to objects in the preceding quote is mainly aimed at objects created from instantiating classes that are coded in an object-oriented programming language, such as Java or .NET. In these platforms, such classes act as a logical representation of the underlying database-table relationships. This approach is typically referred to as Object Relational Mapping (ORM).

In contrast, a Force.com developer reading the term object will tend to first think about Standard or Custom Objects rather than something resulting from a class instantiation. This is because Salesforce has done a great job in binding the data with behavior through their declarative definition of an object, which as we know is much more than just fields. It has formulas, filters, referential integrity, and many other features that we have discussed in earlier chapters, available without writing any code.

Taking into consideration that these declarative features are essentially a means to enable certain behaviors, a Force.com Custom or Standard Object already meets some of the encapsulation concerns of the Domain layer pattern.