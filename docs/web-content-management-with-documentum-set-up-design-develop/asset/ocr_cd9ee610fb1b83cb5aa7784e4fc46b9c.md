API commands can be executed from within Documentum Administrator by clicking on the API button shown in the top right-hand side of the toolbar. This opens the Api Tester screen as shown in figure 26.12.

![](bbox=[0.17, 0.2, 0.83, 0.54])

Figure 26.12: Api Tester utility in Documentum Administrator

# 26.2.2 API Commands

Server API commands follow a defined pattern. You need to type the exact method name followed by arguments as a continuous string with comma as a delimiter, separating the various parts. Here is a sample API command:

```
API> create,c,dm_document
```

Let us break up this command and explain each term:

+   - create: API method name used to create an object instance of the specified object type.
+   - c: Alias for the current session within which the API command is being executed.
+   - dm_document: The argument of the create method. In this case, it is the object type of the object that needs to be created via this API command.

Please keep in mind that the arguments to all the API methods are positional. This means that there are placeholders for all the arguments in an API command even if the argument is optional.