This list can be filtered by typing the beginning of the method's name after the dot. Let's suppose you want to replace some characters in this String instance. As a novice Java programmer, you are not sure if there's a method for that; and if there is, you are not sure which parameters it receives. It's a fair guess that the method's name probably starts with replace, right? So go ahead and type:

```
s.replace
```

When you press Ctrl along with the space bar, you will get a list of all the String() methods whose name starts with replace. By choosing one of them and pressing Enter, the editor completes the code with the rest of the method's name and its parameters. It will even suggest some variables in your code that you might want to use as parameters, as shown in the following screenshot:

![](bbox=[0.23, 0.35, 0.77, 0.53])

Content assist will work with all classes in the project's classpath. You can disable content assist's automatic activation by unmarking Enable auto activation inside the Preferences window and navigating to Java | Editor | Content Assist.

# Code navigation

When the project you are working on is big enough, finding a class in the Package Explorer can be a pain. You will frequently find yourself asking, "In which package is that class again?". You can leave the source code of the classes you are working on open in different tabs, but soon enough you will have more open tabs than you would like to have.