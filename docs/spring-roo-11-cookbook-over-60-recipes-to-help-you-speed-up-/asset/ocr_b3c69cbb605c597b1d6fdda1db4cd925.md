```
findFlightsByDestinationAndOriginEquals(String destination, String origin)
.....
```

As the output suggests, two persistent fields now form part of listed candidate dynamic finder method names. Similarly, if you want your finders to span a n number of persistent fields, then specify n as the value of a depth argument.

# Limiting list of dynamic finder methods, based on a filter criteria

As you increase the value of the depth argument, the number of candidate dynamic finder method names listed by Roo increases exponentially due to the number of possible combinations for method arguments. In such cases, it is desired to filter candidate dynamic finder method names based on a filter criteria. The finder list command provides a filter argument, which accepts a comma separated list of strings that must be present in the dynamic finder method name. Dynamic finder method names which don't contain the strings specified by the filter argument are omitted from the displayed list of candidate dynamic finder method names. This makes it easy for you to locate dynamic finder methods that you want to add to a persistent entity.

> Note that there should be no spaces in the comma-separated list of strings specified as the value of the filter argument.

The following finder list command shows the filter argument usage:

```
~.domain.Flight roo> finder list --depth 2 --filter destinationlike,originlike
```

```
findFlightsByDestinationLikeAndOriginLike(...)
findFlightsByDestinationLikeOrOriginLike(...)
findFlightsByOriginLikeAndDestinationLike(...)
findFlightsByOriginLikeOrDestinationLike(...)
```

The output now shows only four methods that contain the text specified in the filter argument. Similarly, you can add additional text to the filter argument to narrow down the list of candidate dynamic finder method names that are displayed by the finder list command.

# See also

+   - Refer to the Adding dynamic finder methods to an entity recipe below to see how Spring Roo simplifies adding dynamic finder methods to an entity