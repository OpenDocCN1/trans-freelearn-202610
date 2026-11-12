And change it to this line:

```
<directory>./module/*/test/<directory>
```

This may feel like déjà vu because you already did this in Chapter 4, From Object-Relational Mapping to MVC Containers, (at least I hope you did). Now, if you run phpunit again, you will see errors for the two new modules. These errors are related to the routes called by the tests. But for now, we will not fix the test classes. We will do so in Chapter 7, Request Control and Data View.

As the routes for the new modules are not configured yet, the new controllers are not instantiated and therefore, the controllers didn't try to render the template views. But it is important to talk once more about the structure of the view folder for preparing our modules for the next implementations.

The view folder of a Laminas module must be a folder with the name of the module, but only with lowercase letters. The names of folders and files under the view folder are all in lowercase letters. Then, change the application folder under the view folder of the Inventory and Store modules to inventory and store, respectively. Remove the layout folder under the view folder of these modules too. Only one module needs to have a layout folder and the Application module already has one. The appearance of the new modules should be as shown in Figure 5.6:

![](bbox=[0.345, 0.452, 0.612, 0.812])

Figure 5.6 – Structure of the folder module

This is sufficient to start a new Laminas project with custom modules. In the next chapter, we will implement the model layer of our application, using the projected classes in this chapter.