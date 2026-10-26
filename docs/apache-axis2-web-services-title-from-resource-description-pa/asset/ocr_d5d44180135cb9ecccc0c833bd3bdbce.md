# # Filling in the service skeleton

Axis2 generates a service skeleton class with methods that can throw UnsupportedOperation. All that we have to do is implement the service skeleton class according to the business logic you want to provide.

# # Running the ant build file

After completing the service skeleton, the next step is to create a service archive file using the modified generated code. To make this job simple, Axis2 generates an Ant build file to create the service archive file for you. What you have to do is open a command line console and go to the folder where you output the generated code and then type ant build in the console to run the Ant build file. The service archive will now be created for you.

In this chapter, we only skimmed through the contract first approach. You will learn more about code generation and databinding in detail later in this book. We will also be covering a number of samples as well as the available tools.

# # Summary

Turning a Java class into a web service is very straightforward in Axis2. Once you know how to write a `services.xml` file correctly, you can create more complex applications rather than just POJOs. Deploying a service is just a matter of creating a service archive file and dropping it into the services directory in the Axis2 repository. WSDL's first approach is the easiest way of creating a service since Axis2 has inbuilt support for code generation and it has a set of tools to make the job even easier.

In this chapter, we discussed how to convert a simple class file into a web service and how to access that in a REST manner. We discussed what happens when the service becomes a little complex by adding a package name. We also discussed how to use the archive-based deployment mechanism. Finally, we briefly discussed the contract first approach.

In the next chapter, we will discuss how to extend Axis2's core functionality and provide additional service quality using the Axis2 module.