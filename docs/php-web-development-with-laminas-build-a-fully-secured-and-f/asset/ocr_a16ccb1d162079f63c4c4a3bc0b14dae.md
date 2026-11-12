# Summary

In this chapter, we have presented the requirements of our main sample web application: an e-commerce system for a fictional company, Whatstore. We have strived to understand the business rules and requirements of their requested system. We have used some use case diagrams to show at a high level the main flows of the application. In addition, we have presented the projected classes that will serve as inputs for the model layer implementation.

We have also reviewed some steps done in Chapter 2, Setting Up the Environment for Our E-Commerce Application, and Chapter 4, From Object-Relational Mapping to MVC Containers, for the creation of a project with Composer (mediated by Eclipse) and the creation of a new module, using the Application module as a template.

In this chapter, you have learned how to start a complex software project with the Laminas framework. We can understand complexity as a concept related to the number of things in a system and the difficulty in knowing and managing each one of them. According to Cartesian principles, to solve a complex problem, we need to break it into smaller parts. Here, we have defined a modular structure for our software project, dividing the main problem into two parts (two modules). In subsequent chapters, we will learn about division inside modules, which will help us to focus on one small problem at a time.

Contemporary software is complex, and it is necessary to have approaches and tools to deal with complexity. The modular structure of the Laminas framework combined with MVC patterns helps PHP web developers manage complex applications for a distributed systems world.

In the next chapter, we will review what we already learned about the `laminas-db` component, and we will delve into the ORM implementation. We will deal with code reuse when we have to create a generic implementation for our mapper classes.