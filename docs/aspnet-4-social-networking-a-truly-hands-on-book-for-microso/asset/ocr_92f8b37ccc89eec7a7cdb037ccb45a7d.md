An example could be a person in the United States (U.S.). In this case, we could use a person's SSN and each resulting person would be unique. When we look at all the people in the world, the SSN would no longer be considered a good form of unique ID as not all people in the world have one. So in this case, we would probably have to start looking at multiple properties of a person to define their uniqueness. We could take their birth date, last name, country/state, city, and so on. A combination of this information should result in a unique entity in your system. Entities are the most important objects of your domain.

Do all objects in an application need to be unique? Are all objects necessarily entities? Well, the answer is 'No'. In the next section, we will look at value objects as the answer to these questions.

# Value objects

A value object is less important than an entity. It does not require an identity, and hence it can be easily created without being concerned to determine its uniqueness. We are more interested in what the item is rather than who it is. A value object should be immutable. If you need to modify the value object, discard it, and create a new one. But if you find that the object is not immutable, or cannot exist without its own identity, it is most likely to be an Entity object.

To extend our person example from the *Entities* section further, we could make an "address" value object rather than have a person with properties of state, city, zip code, street, or any other information pertaining to the address. The value object would then store information about state, city, zip code, street, and so on. This address object could then be part of a person. So you could use it as *person.Address.City*. Technically, this address could be shared for all the people in the same house. We don't care so much for the address itself, but for the fact that it is attached to a specific person.

Why do we need value objects? Value objects are important as they offer a way of grouping bits of information as you saw in the above example. Being "lightweight" objects, they reduce the amount of resources used in an application. They also simplify an application where value objects don't require uniqueness.

# Services

![](bbox=[0.202, 0.75, 0.275, 0.794])

> The Services term should not be confused with Services from **Service Oriented Architecture (SOA)** world.