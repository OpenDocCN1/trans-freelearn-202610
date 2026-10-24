Our controller is now configured; the next step is to place our dependencies inside our controller. We have used the @AllArgsConstructor annotation. We don’t need to create a constructor method with the parameters; we only need to define dependencies.

First, we will determine the AntiHeroService class where all our CRUD logic is baked. The next one is ModelMapper; this is important, seeing as we need to convert our entity into a DTO when we use it as a response, and vice versa if we want to read an object from a request body. ModelMapper will easily map the values of an entity to a DTO object with the same property.

To install the dependency, we just need to add the following code to our pom.xml file:

```
<!--Dto mapper-->
<dependency>
    <groupId>org.modelmapper</groupId>
    <artifactId>modelmapper</artifactId>
    <version>2.3.9</version>
</dependency>
```

After successfully adding the ModelMapper dependency, we need to define our ModelMapper dependency as a Bean in our configuration to use it on our application. To achieve this, we will create a new package named config under the main package, and we will create a new class called ModelMapperConfig. After creating a new class, we will add a new method annotated with @Bean and return a new instance of ModelMapper. The code is illustrated in the following snippet:

```
@Configuration
public class ModelMapperConfig {

    @Bean
    public ModelMapper modelMapper() {
        return new ModelMapper();
    }
}
```

Our configuration is all done, and now, we can add AntiHeroService and ModelMapper to our controller, as follows:

```
@AllArgsConstructor
@RestController
@RequestMapping("api/v1/anti-heroes")
public class AntiHeroController {
    private final AntiHeroService service;
```