+   4. In the Customer.java file, make the association between the Customer and Address entities as mandatory by setting the optional attribute value of the @OneToOne JPA annotation to false. Also, set the cascade attribute value of @OneToOne to CascadeType.ALL. The following listing shows the modified @OneToOne annotation in Customer.java:

```
import javax.persistence.CascadeType;
..
@RooEntity(table = "CUSTOMER_TBL")
public class Customer {
    @NotNull
    @Column(name = "CUST_NAME")
    private String customerName;
    @OneToOne (optional= false, cascade=CascadeType.ALL)
    @JoinColumn(name = "CUSTOMER_ID")
    private Address address;
}
```

+   5. In the Address.java file, make the association between the Address and Customer entities as mandatory by setting the optional attribute value of the @OneToOne annotation to false. As the Customer entity is the owner of the relationship, set the mappedBy attribute value of the @OneToOne JPA annotation to address. The following listing shows the modified @OneToOne annotation in Address.java:

```
...
@RooEntity(table = "ADDRESS_TBL")
public class Address {
    @NotNull
    @Column(name = "ADDRESS_LINE1")
    private String addressLine1;
    ...
    @OneToOne (optional=false, mappedBy = "address")
    private Customer customer;
}
```

As in Spring Roo 1.1.5, the field reference command doesn't support any argument, which lets you specify the owner of the one-to-one bidirectional relationship; therefore, you need to edit your Java source for specifying the mappedBy attribute value of the @OneToOne annotation. On the other hand, the field set command does provide a mappedBy argument for specifying the relationship owner.

+   6. Replace the CustomerController.java file in the sample.roo.flightapp.web package of SRC_MAIN_JAVA folder with the one contained in the source code folder of this chapter.