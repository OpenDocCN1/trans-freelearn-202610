Let's now look at how Flight entity is modeled.

The following code listing shows noteworthy attributes and methods of the Flight entity:

```
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