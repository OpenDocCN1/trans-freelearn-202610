
## Models and Object-Relational Mappers with Behavior-Driven Development

In this chapter, we will use `laminas-db` to map an entity-relationship model to an object model. Instead of just implementing mappers with Laminas, we will also create a most powerful reusable mapper extending Laminas classes. Finally, we will show how models and mappers are connected. Along with the creation of the classes, we will use the approach of **behavior-driven development** (BDD) to keep user-readable requirements in our source code.

By the end of this chapter, you will have learned how to create models and object-relational mapper classes using the approach of BDD. You will know how to map system requirements to code in a well-organized way. I hope you finish the chapter feeling as satisfied as Luke Skywalker did after exploding the Death Star!

In this chapter, we'll be covering the following topics:

- BDD with Behat
- Creating models from user stories
- Creating mappers from user stories
- Creating a generic model and a generic mapper
- Creating other models with Behat

### Technical requirements

All the user stories implemented for **Behat** for this chapter are fully available at https://github.com/PacktPublishing/PHP-Web-Development-with-Laminas/tree/main/chapter06/whatstore/features. In the same repository, you can find the model classes in the Model folder of the Inventory and Store modules.

### BDD with Behat

In Chapter 3, Using Laminas as a Library with Test-Driven Development, we experienced the TDD approach. TDD is a very important skill to ensure the stability of software. When you practice TDD, you know that all the software continues to work after you change a part of it. Beyond this, the test classes document the business rules, and thus, TDD helps you to maintain useful documentation. A test class with running tests tells you that the rules contained in the tests are enforced by classes or subroutines of the software.

Test cases written in the xUnit pattern could be more readable for non-programmers than the direct implementation of the software, but you are still using a programming language to express business rules. How wonderful would it be if we could write tests in a more human language? BDD provides this. It is an approach that extends TDD, using an abstraction layer over the automated tests, written by and for programmers. It allows the writing of more understandable stories for those who know a business but don't know, or even need to know, the technology they are using to develop their applications.

**The xUnit pattern**
Kent Beck and Erich Gamma created JUnit, a test framework for Java. The JUnit architecture was used as a base for creating test frameworks for other languages such as PHP. There is a family of test frameworks that follow the same pattern for the implementation of unit tests, test cases, and test suites. Kent Beck and other authors such as Gerard Meszaros call this family xUnit, where the letter x is a variable placeholder in a programming language. The common structure of xUnit frameworks is well described by Gerard Meszaros in his book xUnit Test Patterns: Refactoring Test Code (Addison-Wesley).

We will use BDD to implement the model layer for the Whatstore project. We need to install a BDD tool for PHP. There is a BDD implementation for PHP called Behat.

Using the Composer editor of Eclipse, add a development dependency for the behat/behat package. Remember that you already did something like this in Chapter 3, Using Laminas as a Library with Test-Driven Development, when you installed PHPUnit. After adding the package, save the file, and click on the Update dependencies (including require-dev) button to download Behat.

Based on what we have learned in Chapter 2, Setting Up the Environment for Our E-Commerce Application, we will create some external tool configurations for the most common invocations of the Behat executable. Using the Run | External Tools | External Tools Configurations menu, create the following three configurations:

- **Behat initialization:**
  - Name: Behat init
  - Location: ${project_loc}/vendor/bin/behat
  - Working Directory: ${project_loc}
  - Arguments: --init

- Running of the behavior tests:
  - Name: Behat run
  - Location: ${project_loc}/vendor/bin/behat
  - Working Directory: ${project_loc}
  - Arguments: Leave this empty

- Snippet appending:
  - Name: Behat append snippets
  - Location: ${project_loc}/vendor/bin/behat
  - Working Directory: ${project_loc}
  - Arguments: --append-snippets

Don't forget, as we did in Chapter 2, Setting Up the Environment for Our E-Commerce Application, to check the External Tools item on the Common tab for each one of these configurations.

Now that we have avoided unnecessary command typing for the next steps, we can initialize Behat for the Whatstore project. Select the project name in the Project Explorer view and click on the Behat init item on the external tools list in the toolbar, or in the Run | External Tools menu. The output on the Console tab will be as follows:

```
+d features - place your *.feature files here
+d features/bootstrap - place your context classes here
+f features/bootstrap/FeatureContext.php - place your definitions, transformations and hooks here
```

> Variable references empty selection: ${project_loc}
> 
> If the Variable references empty selection: ${project_loc} message appears after you run any external tool, it is because you haven't selected the project name in the Project Explorer view. Select the Whatstore project name and run it again.

Press F5 or click on the Refresh item of the project to view the new features folder in the Project Explorer view. The files with the .feature extension contain user stories, with a language closer to the human language than a programming language. We can say that it seems like a normal people language. The bootstrap folder will contain classes with code related to the stories defined by the .feature files. Don't worry – we will explain how to create stories.

Select the project name in the **Project Explorer** view and click on the **Behat run** item on the external tools list in the toolbar, or in the **Run/External Tools** menu. The output on the **Console** tab will be this:

```
No scenarios
No steps
0m0.00s (8.67Mb)
```

There are no scenarios because there are no files with the .feature extension inside the features folder. Let's create our first scenario to understand what it is in BDD.

### Creating our first scenario

In the preceding chapter, we presented several use cases. Some of them were related to the management of products by employees. Let's start with the use case *product addition*. A use case is, in fact, a user story that expresses a desired feature for a software system. So, from the use case *product addition*, we will write a file named productaddition.feature that will describe this feature. Create this file through the File | New | File menu. Its content will be the following:

### productaddition.feature

```
Feature: Product addition
    In order to have products in the stock
    As an employee
    I need to be able to add a product to the product
    registration

    Rules:
    - The code of product is generated automatically
    - Product name has a maximum of 80 characters
    - A product must have a discount, and the default
      discount is the null discount

    Scenario: Inserting a product in the registration
    Given there is a product called "Troop Power Battery",
    which costs $2814
    When I add this product to the registration
    Then I should have a product called "Troop Power
    Battery" in the table products
    And the overall product price should be $2814
```

Let’s understand the patterns used for the productaddition.feature file. The file begins with the word Feature. A feature is another name for a user story or use case. The three lines that follow have initial words that must be present for every feature description:

- In order...: The goal of the user story
- As a/an...: The role played by the story actor
- I need...: The condition for the actor to reach the goal

These initial words define a pattern for describing a feature. Optionally, you can add a Rules section to describe the business rules related to the feature. This first part of the .feature file is only descriptive and works like any other document of requirements. The second part, on the other hand, serves to generate source code.

A scenario describes a flow using an example. So, we have real values instead of variables. You can create several blocks, starting with the word Scenario, according to the possible flows. A Scenario block has phrases that start with the following words:

- Given...: The condition for the flow to start
- When...: The action that starts the flow
- Then...: The result of the flow
- And (optional)...: An additional result of the flow

All these four keywords are part of a language projected to give structure and meaning to executable specifications – Gherkin. Gherkin is part of the common language analyzer of Cucumber, a BDD tool for Java. You can learn more about Gherkin at this URL: https://cucumber.io/docs/gherkin/reference.

The phrases of a scenario are used to create methods of a context class. A context class allows you to map a user story to an automated test. Let’s see how it works.

### Mapping a user story to an automated test

As you have seen, we wrote a scenario where an employee inserts a product called "Troop Power Battery" that costs $2,814. As the type of discount was not specified, we assume that the discount is null. Okay, but how does this scenario become an automated test? You will discover this by selecting the whatstore project in the Project Explorer view and clicking on the Behat run item in the Run | External Tools menu. The output on the Console tab will have the content of the productaddition.feature file added with the following lines:

```
1 scenario (1 undefined)
4 steps (4 undefined)
0m0.01s (8.89Mb)
```
