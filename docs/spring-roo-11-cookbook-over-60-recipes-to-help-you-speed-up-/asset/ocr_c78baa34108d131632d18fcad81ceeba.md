```
public static final Country GERMANY =
new Country("Germany");

private String countryName;

public Country(String countryName) {
this.countryName = countryName;
}

public String getCountryName() {
return countryName;
}
}
```

The preceding code shows that the Country class defines constants for each country representing a possible value of the countryOfOrigin argument. Now, to use the Country class instead of the MysimplePropertyName enum, all you need to do is to replace references to it with Country in the MysimpleCommands class, as shown here:

```
public class MysimpleCommands implements CommandMarker {
@Reference private StaticFieldConverter staticFieldConverter;

protected void activate(ComponentContext context) {
staticFieldConverter.add(Country.class);
}

protected void deactivate(ComponentContext context) {
staticFieldConverter.remove(Country.class);
}
```

```
@CliCommand(value = "say hello",
help = "Prints welcome message to the Roo shell")
public void sayHello(... , @CliOption(key = "countryOfOrigin",
mandatory = false,
help = "Country of origin") Country country) {

log.info("Welcome " + name + "!");
log.warning("Country of origin: " + (country == null ?
Country.NOT_SPECIFIED.getCountryName() :
country.getCountryName())) ;
...
}
...
```