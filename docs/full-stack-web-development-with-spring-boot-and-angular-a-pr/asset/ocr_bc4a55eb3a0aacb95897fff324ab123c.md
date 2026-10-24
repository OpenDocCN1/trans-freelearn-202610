```
private String knownAs;
private String createdAt =
    new SimpleDateFormat("dd-MM-yyyy HH:mm:ss z")
        .format(new Date());
}
```

In the preceding code block, we can see that we have added a PK of the UUID type; this will be automatically generated once we insert new anti-hero data into our database. The type is defined using the @GeneratedValue annotation, where we have also indicated that the strategy will be using an auto generator. We have also added several fields that will be used to store the anti-hero information.

We have successfully created our entity; now, we will create a DTO for the anti-hero entity. Right-click on the dto package, select New | Java class, and we will name the new class AntiHeroDto. After successfully creating the DTO, we will place the following code into it:

```
package com.example.superheroes.antiHero.dto;

import java.util.UUID;
import javax.validation.constraints.NotNull;
import lombok.Getter;
import lombok.Setter;

@Getter
@Setter
public class AntiHeroDto {

    private UUID id;

    @NotNull(message = "First Name is required")
    private String firstName;

    private String lastName;
    private String house;
    private String knownAs;
}
```

In the preceding code example, we have added fields that we only want to display as we send the anti-hero data to the client; in this case, we have removed the createdAt field in our DTO. We have also added @Getter and @Setter annotations to generate getter and setter methods for our DTO, and lastly, we have also added @NotNull validation to match our constraint with the entity.