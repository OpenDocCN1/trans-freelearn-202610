
## Creating a launch configuration

If your project needs customizations, such as arguments or a specific classpath configuration to run, you can create a launch configuration to meet these needs. For this, navigate to Run | Run Configurations. The following screenshot shows the launch configuration window:

![](img/d7c3f2018603810fae83e1a6fd98749e_54_0.png)

On the left-hand side of the Run Configurations window, you have a list of all the launch configuration categories. To create a new launch configuration for a Java project, double-click on the Java Application category. If you have already run your application once with the Run button, there will be a default configuration in this category.

By selecting the configuration, you will be presented with the tabs that contain the launch configurations. A brief description of what each tab contains is as follows:

- **Main**: This is where you will choose the project and the main class. The **Search** button beside the **Main class** text field allows browsing through all the classes with a main method within the project.
- **Arguments**: This tab lets you set arguments for both the main class itself and for the JVM.
- **JRE**: This tab lets you choose the JRE that will be used to run the class.
- **Classpath**: Edit this tab to create a launch configuration with a specific classpath configuration. Modifying this tab's configurations doesn't modify the project's classpath configuration.
- **Source**: This tab lets you change the source lookup path. As in the **Classpath** tab, these configurations don't modify the project itself and are only valid for this launch configuration.
- **Environment**: This tab allows you to define environment variables for this run configuration.
- **Common**: This tab contains the general configuration of the launch. It allows you to choose to allocate a console for the launch and write the output to a file instead of the console. The **Save as** section refers to the launch configuration file.

## Managing the project build path

The build path is a list of JARs, classes, and source file folders in which Eclipse will look to resolve dependencies. You can edit the build path for a project by entering the project's properties and selecting **Java Build Path**. The configuration options have been divided into four tabs:

- **Source**: This contains options to manage the project's source folders. It allows you to create new source folders and add links to external sources in the filesystem.
- **Projects**: You can manage your project's dependency on other projects in your workspace in this tab. When you'll add a dependency to another project, you'll have full access to all of its classes by default. However, some of these classes might not be visible when the project is compiled and executed outside Eclipse. You might end up with a project that works inside Eclipse but doesn't work when you run the JAR files outside it. To avoid these head-scratching issues, you can create access rules to external projects to keep them from using other projects' internal packages. For this, expand the required project, choose **Access Rules**, and click on **Edit**. Click on **Add Access Rule** and add **Rule Pattern**. You can discourage (compiles but shows a warning) or forbid (compiling fails with an error) access.
- **Libraries**: In this tab, you'll be able to edit the libraries that will be added to the project's build path. You can add JARs and class folders from inside or outside the workspace, and you can also add libraries and variables. You can add a link from a library to its Javadoc. This will allow you to have methods and classes with comments that describe their usage, as if they were in your workspace. To add a Javadoc for a library, expand its entry in the list, select **Javadoc location**, and click on the **Edit** button. Enter the path for the Javadoc and click on **OK**. You can also point to the library's source if you have access to it.
- **Order and Export:** This is where you'll define which source folders and libraries from the project will be exported so that other projects can import them. This tab also allows you to choose the order in which the libraries will be loaded during runtime. The order will determine which class will be loaded in case two different libraries provide that same class. To increase or decrease a library's priority, select it and press the **Up** and **Down** buttons.

## Summary

In this chapter, we have covered the basics of Java developing with Eclipse: creating and importing a Java project, organizing projects in working sets, adding new classes, among others. We went through all of Eclipse's views for Java developing, learned how they can be used to navigate through code, found errors, warnings, and much more. Special attention was given to the Java Editor by covering its most useful features. We also covered some more advanced topics, such as creating templates for code generation and project settings, that can be shared among developers in a team to enforce code style and best practices.

In the next chapter, you will learn how Eclipse can help you debug your Java applications as well as test them using JUnit.

## 3 Unit Testing with JUnit and Debugging

By now, you are past the basics and should be familiar with developing Java applications using the Eclipse IDE. Although you must be feeling very confident with your recent acquired skills, before we go any further, you should know that the more you code, the higher are the chances of you introducing a bug into your code. Even with all the facilities provided by Eclipse, things are bound to go wrong as Java applications scale up. So, we need to build a safety net whenever our application code starts to become nontrivial to manage. One way of doing this is to test our programs, and testing is one of the things that we are going to cover in this chapter.

In an ideal world, all problems detected by our tests would be easy to track down and fix. Nevertheless, this is not always the case. It turns out that nasty bugs might make their way into our code. As mentioned, having a good set of tests helps us to spot problems, however, sometimes the feedback provided by the test set is not enough, and we need to dig deeper to uncover problems. The act of understanding why the software is not behaving properly and working out a fix for it is called **debugging**. Debugging is covered later in this chapter.

When it comes to testing and debugging Java programs, Eclipse has got you covered. It comes with built-in features for automating the creation and execution of JUnit tests, and it features an impressive Java debugger.

We will cover the following topics in this chapter:

- Configuring JUnit inside Eclipse
- Organizing, developing, and running unit tests using JUnit inside Eclipse
- Using Eclipse's built-in debugger when bugs arise and understanding that only test methods are not enough to pinpoint the problem cause

## Testing with JUnit – getting started

Before describing JUnit, we believe that some words about unit testing are in order. When carrying out unit testing activities, emphasis is given to small chunks of code. In other words, you are not trying to cover the whole program with a single test. Rather, you are testing the foundations of your programs by writing a suite of tests per class, one or more tests per method. Writing tests while you are working on the code at hand has a host of benefits. For instance, it is easier to find bugs when unit testing. This is the case because each unit test should evaluate exactly one functional unit, which makes the scope one has to hunt for bugs significantly smaller than searching the whole program for bugs. However, according to Robert C. Martin, to reap the full benefits of unit testing, your test suite has to comply with the following principles (also known as the FIRST acronym):

- **Fast**: The tests should be fully automatic and should be able to check their own results. But most importantly, you should be able to execute them fast so that they can be frequently executed.
- **Independent**: The tests should not depend on each other. You should be able to run your tests in any particular order.
- **Repeatable**: You should be able to execute your tests in any environment (not only production) and as frequently as required.
- **Self-validating**: The tests should produce a Boolean outcome, that is, they should either pass or fail; there should be nothing in between. In fact, it boils down to avoiding subjective outputs that require manual evaluation.
- **Timely**: According to Robert C. Martin, unit tests should be written before the code implements the feature that they evaluate. According to him, it is harder to write tests after implementing the production code. From an academic perspective, there is no substantial evidence regarding the benefits of writing tests before, in comparison to writing tests after implementing the code that they evaluate. This is clearly a controversial topic, we do not seek to influence you in favor of a particular approach. Try writing tests before and after implementing the code and see which works best for you.

![](img/d7c3f2018603810fae83e1a6fd98749e_58_0.png)

> In case you think that writing tests before implementing code suits you better, you should read about Test Driven Development (TDD). While you are at that, you might also want to study refactoring, which consists of improving your code without changing its external behavior.
