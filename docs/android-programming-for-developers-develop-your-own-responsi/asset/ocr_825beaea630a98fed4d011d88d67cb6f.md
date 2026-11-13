# # The NameAndAddress class

Create a new class and call it NameAndAddress. We implement Serializable as this is required by ArrayAdapter, which will act as our adapter between ListView and our data. We will see this in action soon. We will see that ArrayAdapter saves us a mountain of code at the expense of a slight loss in flexibility.

Add this code for the NameAndAddress class:

```
public class NameAndAddress implements Serializable{

private String mName;
private String mAddress1;
private String mAddress2;
private String mZipCode;

public NameAndAddress(String name,
String address1,
String address2,
String zipCode){

mName = name;
mAddress1 = address1;
mAddress2 = address2;
mZipCode = zipCode;
}
}
```

We need a getter method for each member, so you can use one of these three methods:

+   1. Type the next code block.
+   2. Copy and paste it from the download bundle.
+   3. Right-click below the last line of code and navigate to Generate | Getters.
+   Then, highlight all the members and click on OK. This generates getter methods with names such as getmName that you will need to tidy up the names to match the more friendly method names that we will be using, such as getName, as per the next code block.

Here is the code that needs to be added to the NameAndAddress class using the