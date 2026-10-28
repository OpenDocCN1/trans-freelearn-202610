# 15

# Get Data into Table Views

In the previous chapter, you learned about the Model-View-Controller (MVC) design pattern and table views. You also reviewed the table view in the Journal List screen. At this point, the Journal List screen displays cells that do not contain any data. As shown in the app tour in Chapter 10, Set Up the User Interface, it should display a list of journal entries.

In this chapter, you’re going to implement the model objects for the Journal List screen to make it display a list of journal entries. You’ll start by learning about the requirements for the model objects that you will use. Then, you’ll create a Swift class that can store journal entry instances. After that, you’ll create a static method that returns an array containing sample journal entry instances. This array will then be used as the data source for the table view on the Journal List screen.

By the end of this chapter, you’ll have learned how to create model objects, how to create sample data, and how to configure view controllers to populate table views.

The following topics will be covered in this chapter:

+   - Model objects
+   - Create a class to represent a journal entry
+   - Create sample data
+   - Display data in a collection view