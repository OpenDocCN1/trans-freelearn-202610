The CSS hover class is set to scale the slide when the mouse enters so a slide can be focused on for review. The classes are set with the toggle method, which either adds if not present or removes if they are. This method has an optional parameter:

```
aSlide.classes.toggle('className', condition);
```

The second parameter, named shouldAdd, is true if the class is always to be added and false if the class is always to be removed.

# Handout notes

There is nothing like a tangible handout to give presentation attendees. This can be achieved with a variation of the overview display:

![](bbox=[0.215, 0.365, 0.785, 0.66])

Instead of duplicating the overview code, the function can be parameterized with an optional parameter in the method declaration. This is declared with square brackets [] around the declaration and a default value that is used if no parameter is specified:

```
void buildOverview([bool addNotes = false])
```