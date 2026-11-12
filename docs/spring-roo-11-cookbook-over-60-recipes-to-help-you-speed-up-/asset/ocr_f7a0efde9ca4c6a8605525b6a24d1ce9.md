The following table describes the important directories and files that are created when the flex remoting all command is executed:

| Directory / file | Description |
| :--- | :--- |
| SRC_MAIN_JAVA\\sample\\roo\\flightapp\\flex | Contains remoting destinations created by Roo corresponding to each JPA entity in the flightapp_flex application for which a remoting destination doesn't exist. This directory is created based on the package argument value of the flex remoting all command. |
| ROOT\\src\\main\\flex\\sample\\roo\\flightapp\\domain | Contains Roo-generated ActionScript classes that map to JPA entities in the flightapp_flex application. |
| ROOT\\src\\main\\flex\\sample\\roo\\flightapp\\presentation\\flightdescription | Contains MXML files and ActionScript classes for performing CRUD operations on the FlightDescription JPA entity. The name of the directory is derived from the name of the JPA entity. |
| ROOT\\src\\main\\flex\\sample\\roo\\flightapp\\presentation\\flight | Contains MXML files and ActionScript classes for performing CRUD operations on the Flight JPA entity. The name of the directory is derived from the name of the JPA entity. |

When the flex remoting all command is executed, it creates a remoting destination (which is also Spring's service component) corresponding to each JPA entity in the application for which a remoting destination doesn't already exist. A remoting destination defines methods to perform CRUD operations on the corresponding JPA entity. The following code listing shows the remoting destination, FlightDescriptionService class, created by Roo corresponding to the FlightDescription JPA entity:

```
FlightDescriptionService.java
package sample.roo.flightapp.flex;

import org.springframework.flex.roo.addon.RooFlexScaffold;
import sample.roo.flightapp.domain.FlightDescription;
import org.springframework.flex.remoting.RemotingDestination;
import org.springframework.stereotype.Service;

@RooFlexScaffold(entity = FlightDescription.class)
@RemotingDestination
@Service
public class FlightDescriptionService {
}
```