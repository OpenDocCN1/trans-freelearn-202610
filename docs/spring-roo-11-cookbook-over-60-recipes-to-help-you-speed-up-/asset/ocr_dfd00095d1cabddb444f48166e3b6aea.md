The given screenshot shows the data for the `FlightDescription` kind that we created. You can use the **Query** tab to query the datastore using **GQL**. You can also delete or edit the entity instances. If you change `kind` to `Flight`, then the Admin Console will show the `Flight` entities that you have created, as shown here:

| | ID/Name | VERSION | flightName |
|---|---|---|---|
| □ | name=FLT-100 | 1 | FLT-100 |
| □ | name=FLT-200 | 1 | FLT-200 |
| □ | name=FLT-300 | 1 | FLT-300 |
| □ | name=FLT-400 | 1 | FLT-400 |

The given screenshot shows that the `name` set for the primary key of the `Flight` instance is displayed under the **ID/Name** column.

# There's more...

If you are designing your entities for App Engine, it is important to note that within a transaction you can only operate on entities which belong to the same entity group. So, if you have `Student` and `Course` entities, which are in different entity groups, then you can't create or update them in a single transaction.

As of Spring Roo 1.1.3, you can only create _unowned_ relationship between entities. For instance, if you use the `field reference` or `field set` command to add a relationship field, then the `*_Roo_JavaBean.aj` AspectJ ITD file corresponding to the JPA entity removes the `@OneToMany` or `@OneToOne` or `@ManyToOne` or `@ManyToMany` annotation from the corresponding field in the JPA entity Java source file, resulting in an _unowned_ relationship. The following code fragment shows how the AspectJ ITD file removes the relationship annotation from Java source file:

```
privileged aspect FlightDescription_Roo_JavaBean {

declare @field: * FlightDescription.flights: -@OneToMany;
...
}
```