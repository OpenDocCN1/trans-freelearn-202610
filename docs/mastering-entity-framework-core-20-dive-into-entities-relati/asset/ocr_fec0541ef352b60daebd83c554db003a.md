We have seen the proposed design of the blogging system; now let's learn about the relationships and how they could be built using the same design.

# The one-to-one relationship

We need a new entity to explain the one-to-one relationship, as the existing entities don't have the provision to explain the same. So let's pick the Address entity from the proposed design and see how the one-to-one relationship is built:

The code illustrating one-to-one relationship between User and Address entities was listed as follows:

```
public class User
{
    public int Id { get; set; }
    public string DisplayName { get; set; }
    public string Username { get; set; }
    public string PasswordHash { get; set; }
    public string Email { get; set; }
    public DateTime CreatedAt { get; set; }
    public DateTime ModifiedAt { get; set; }
    public int CreatedBy { get; set; }
    public int ModifiedBy { get; set; }
    public ICollection<Blog> Blogs { get; set; }
    public ICollection<Post> Posts { get; set; }
    public ICollection<Comment> Comments { get; set; }
    public int? AddressId { get; set; }
    public Address Address { get; set; }
    public int PersonId { get; set; }
    public Person Person { get; set; }
}

public class Address
{
    public int Id { get; set; }
    public string FlatHouseInfo { get; set; }
    public string StreetName { get; set; }
    public string Locality { get; set; }
    public string City { get; set; }
    public string State { get; set; }
    public string Country { get; set; }
    public string LatitudeLongitude { get; set; }
    public DateTime CreatedAt { get; set; }
    public DateTime ModifiedAt { get; set; }
    public int CreatedBy { get; set; }
```