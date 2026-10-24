+   - **Repository:** This is an interface that we need to extend with JPA repositories for the entities to have built-in operations.

In the previous example, we have a Villain entity. To implement the CRUD operations, we will create a VillainRepository interface and extend it with CrudRepository, with a type of Villain and UUID:

```
@Repository
public interface VillainRepository extends
CrudRepository<Villain, UUID> {

    // custom composite repository here
}
```

+   - **Service:** This is where we will use our created repository. We can use the @Autowired annotation to inject the repository and call the JPA and custom-defined methods:

```
@Service
public class VillainService {
private final VillainRepository villainRepository;

@Autowired
public VillainService (VillainRepository
villainRepository) {
    this. villainRepository = villainRepository;
}

    public Iterable<Villain> findAllVillains() {
        return villainRepository.findAll();
    }

    public Villain findVillainById(UUID id) {
        return findOrThrow(id);
    }
}
```

In the preceding example, we can see that we have injected VillainRepository in VillainService using the @Autowired annotation.