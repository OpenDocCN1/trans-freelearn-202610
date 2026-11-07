We add a field named genre to the book table and define it as an ENUM. For now, we choose to put short codes in the value list and make one of them, F, into the default value as shown in the following screenshot:

![](bbox=[0.18, 0.205, 0.82, 0.395])

In the value list, we have to enclose each value within single quotes, unlike in the default value field. In our design, we know that these values stand for Fantasy, Child, and Novel. However, for now, we want to see the interface's behavior with short code. In the Insert panel, we now see a radio box interface.

![](bbox=[0.18, 0.495, 0.82, 0.54])

If we decide to have more self-describing code, we can go back to Structure mode and change the values definition for the genre field. We also have to change the default value to one of the possible values, to avoid getting an error message while trying to save this field structure's modification.

![](bbox=[0.265, 0.635, 0.735, 0.78])