On successful authentication, we will be seeing the following highlighted modules visible on the screen, the first one is the *Welcome* message of the user and the other is the admin links visible in the navigation bar:

![](bbox=[0.105, 0.191, 0.894, 0.404])

Now we are good to go, the system can now support row-level security, which is discussed in the next section.

# ## Row-Level Security

The **Row-Level Security (RLS)** will allow us to maintain data from all users in a single database, but still ensure that only data related to the user is shared and the other user's content is kept hidden. The underlying logic could be implemented in any approach, which is left to the developer's creativity, but we will stick with tenants since we will be dealing with multi-tenancy in the system. Let's create a `Tenant` type using the following code snippet that can accommodate a person's information along with their tenant ID and name:

```
```csharp
public class Tenant
{
    public Guid Id { get; set; }
    public string Name { get; set; }
    public Person Person { get; set; }
}
```
```