```
return
(FlightDescription)entityManager().createQuery("select o
from FlightDescription o where o.flightDescriptionId =
:id").setParameter("id",
KeyFactory.stringToKey(key)).getSingleResult();
}
...
public String getFlightDescriptionKeyAsString() {
return KeyFactory.keyToString(flightDescriptionId);
}
...
}
```

The given code shows that FlightDescription entity's primary key is of type com.google.appengine.api.datastore.Key. The Key not only holds the entity's primary key, but also holds information about the entity group to which the entity instance belongs. As the FlightDescription represents a root entity (that is, without a parent), the Key field will not contain entity group information. The @OneToMany annotated flights field defines an owned relationship between the FlightDescription and Flight entities.

If it was an unowned relationship the flights field would have taken the following form:

```
private List<Key> flights;
```

Here, the flights field refers to the primary keys of Flight entity instances. Also, the field is not annotated with the @OneToMany annotation.

The findFlightDescription method of the FlightDescription entity takes encoded String value of Key to find the matching FlightDescription. The KeyFactory class provides keyToString and stringToKey to convert Key into its encoded String form and vice versa. This is particularly useful when you are creating relationships between entities via the user interface of the web application. The query to fetch the FlightDescription object makes use of the KeyFactory class to obtain Key from its String representation and use it as part of the query, as shown here again:

```
entityManager().createQuery("select o
from FlightDescription o where o.flightDescriptionId =
:id").setParameter("id",
KeyFactory.stringToKey(key)).getSingleResult();
```

The persist method of FlightDescription shows that you don't need to set the value of the primary key. It is set by the App Engine.

The getFlightDescriptionKeyAsString method of the FlightDescription entity makes use of KeyFactory class to return the primary key of the entity as an encoded String value.