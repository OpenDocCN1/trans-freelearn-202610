
In an unowned relationship, each entity instance exists independently of each other. For example, the relationship between Student and Course entities is an example of an unowned relationship. A Student entity can exist without a Course and a Course entity can exist without a Student.

In our Flight Booking application, the relationship between FlightDescription and Flight entities is an example of an owned relationship, where Flight entity instances cannot exist without a FlightDescription instance. Let’s now see how an owned one-to-many relationship has been created between the FlightDescription and Flight entities in the Flight Booking application.

## Owned relationship

The following code listing shows some of the important methods and attributes of the FlightDescription entity:

```java
import com.google.appengine.api.datastore.Key;
import com.google.appengine.api.datastore.KeyFactory;

@Entity
public class FlightDescription {
	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	@Basic
	private Key flightDescriptionId;

	@Basic
	@NotNull
	private String origin;
	...

	@OneToMany
	private List<Flight> flights;

	...

	@Transactional
	public void persist() {
		if (this.entityManager == null)
			this.entityManager = entityManager();
		this.entityManager.persist(this);
	}

	@Transactional
	public static FlightDescription
	findFlightDescription(String key) {
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

Let's now look at how Flight entity is modeled.

The following code listing shows noteworthy attributes and methods of the Flight entity:

```java
@Entity
public class Flight {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Basic
    private Key flightId;

    @Basic
    @NotNull
    private String flightName;

    private transient String encodedFlightDescriptionId;

    @Transactional
    public void persist() {
        if (this.entityManager == null)
            this.entityManager = entityManager();

        Key parentKey = KeyFactory.stringToKey(
            encodedFlightDescriptionId);

        Key flightKey = KeyFactory.createKey(parentKey,
            Flight.class.getSimpleName(), flightName);

        setFlightId(flightKey);
        this.entityManager.persist(this);
    }

    public String getEncodedFlightDescriptionId() {
        return encodedFlightDescriptionId;
    }

    public String getFlightKeyAsString() {
        return KeyFactory.keyToString(flightId);
    }
}
```

In the given code, `flightId` represents the primary key of the `Flight` entity. The `persist` method of the `Flight` entity makes use of the `createKey` method of the `KeyFactory` class to create the primary key. The `createKey(parent, kind, name)` method accepts primary key information of the **parent** entity (`FlightDescription` is the parent in the case of `Flight` entity), the **kind** (which is similar to the concept of tables in relational databases), and the **name** of the key that uniquely identifies the entity within the **kind**. In the case of the `Flight` entity's primary key, the `kind` refers to the simple name of the entity class: `Flight.class.getSimpleName()` and the `name` of the key is the `flightName` property of the `Flight` entity. So, if you re-create a `Flight` entity with `flightName` as `FLT-100`, then it will overwrite the existing `Flight (FLT-100)` entity instance. GAE datastore doesn't support a composite primary key, but you can still achieve it by concatenating multiple entity field values in creating the `name` of the key.

The `encodedFlightDescriptionId` is the `String` form of the `FlightDescription` primary key who is the parent of `Flight` entity instance. The value of `encodedFlightDescriptionId` is set when the user selects the `FlightDescription` from the user interface for creating the `Flight` instance, as shown here:

![](img/b8c379a2ed921ead3aa2b01811dc29fa_362_0.png)

The `<option>` element corresponding to the `FlightDescription (NYC-DELHI-1200)` entity instance is rendered as shown here:

```
<option value="<string value of Key>">NYC-DELHI for only 1200 USD</option>
```

The `value` attribute specifies the `String` form of the `FlightDescription` primary key (which is of type `Key`). So, when the user clicks the **Create** button, the `String` value of `Key` is bound to the `encodedFlightDescriptionId` field of the `Flight` instance.

The `flightapp-gae-spring-mvc` project contains the `KeyEditor` property editor class that uses `KeyFactory` to perform conversion from `Key` to `String` format and vice versa. The property editor has been used instead of a Spring `ConversionService` implementation because the `ConversionService` implementation doesn't currently work with the `<options>` tag of the Spring `form` tag library.

Let's now look at how we can manage persisted data in GAE datastore using Admin Console.

## Managing persisted data using Admin Console

You can view, edit, and delete data persisted by your application in GAE datastore by using Admin Console. Go to http://appengine.google.com and sign in to view the applications that you have created, as shown here:

![](img/b8c379a2ed921ead3aa2b01811dc29fa_363_0.png)

The given screenshot shows the two applications that I have created in GAE. The roo-cookbook-spring-mvc application corresponds to the application that we saw in this recipe. Selecting the application will show the Dashboard for the application where you can view different statistics related to your application. We are only interested in viewing the data that we saved in the datastore, so select the Datastore Viewer option from the Dashboard. The Datastore Viewer shows the entities that you saved in the datastore, as shown in the following screenshot:

![](img/b8c379a2ed921ead3aa2b01811dc29fa_363_1.png)

The given screenshot shows the data for the `FlightDescription` kind that we created. You can use the **Query** tab to query the datastore using **GQL**. You can also delete or edit the entity instances. If you change `kind` to `Flight`, then the Admin Console will show the `Flight` entities that you have created, as shown here:

| □ | ID/Name | VERSION | flightName |
|---|---|---|---|
| □ | name=FLT-100 | 1 | FLT-100 |
| □ | name=FLT-200 | 1 | FLT-200 |
| □ | name=FLT-300 | 1 | FLT-300 |
| □ | name=FLT-400 | 1 | FLT-400 |

The given screenshot shows that the `name` set for the primary key of the `Flight` instance is displayed under the **ID/Name** column.

## There's more...

If you are designing your entities for App Engine, it is important to note that within a transaction you can only operate on entities which belong to the same entity group. So, if you have `Student` and `Course` entities, which are in different entity groups, then you can't create or update them in a single transaction.

As of Spring Roo 1.1.3, you can only create _unowned_ relationship between entities. For instance, if you use the `field reference` or `field set` command to add a relationship field, then the `*_Roo_JavaBean.aj` AspectJ ITD file corresponding to the JPA entity removes the `@OneToMany` or `@OneToOne` or `@ManyToOne` or `@ManyToMany` annotation from the corresponding field in the JPA entity Java source file, resulting in an _unowned_ relationship. The following code fragment shows how the AspectJ ITD file removes the relationship annotation from Java source file:

```
privileged aspect FlightDescription_Roo_JavaBean {

declare @field: * FlightDescription.flights: -@OneToMany;
...
}
```
