5. Like the previous example, type `mvn clean test` to execute the scenario tests that are already in this model – these samples are the Scenario tests for Traffic Violation decision model, which is pretty clear to understand since we talked about it in detail in *Chapter 10*.

6. Like our previous example, you will get a summary (in the terminal) and details (in the Surefire reports) of the scenario test progress – for the Kogito examples, we’d expect all the tests to pass at this point.

Now that we have a project set up to run scenario tests, we can add the decision model (Bill of Materials) and test scenario to the same project locations as described in the previous example. VSCode online supports dragging and dropping files from your laptop to add them to the project.

You do not need to edit `pom.xml`, nor add the Java test file, as these steps are already contained in the project.

At some point, you will probably end up modifying the Kogito samples as the basis for your own project. We encourage you to do this, as while it’s outside the scope of the book, it is a step toward the way that Java developers interact with decision models and business rules.

# Running VSCode on your laptop

As we’ve just seen, running VSCode in the Azure cloud and Codespaces is very convenient. While there is some setup, a lot of the tools are preconfigured for you. But sometimes, the tools don’t have the latest update, which might cause KIE to throw an error if it expects a different version.

For whatever reason, running VSCode on your laptop is very achievable – but you will need to make sure the following software tools are installed on your laptop before you work through the preceding steps:

+   - VSCode from Microsoft
+   - Java (**Java Development Kit (JDK)**)
+   - Maven, the build tool from Apache

We haven’t covered the installation of these tools on a laptop since they are not needed for the cloud version, and they are freely available with full install instructions on the respective websites.