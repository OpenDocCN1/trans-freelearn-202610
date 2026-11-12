
Roo provides many built-in services which simplify add-on development. FileManager and ProjectOperations types represent services provided by Roo for managing files (like creating, reading, updating files, undo capability, and so on) and performing actions on the Roo project (like adding dependencies to the pom.xml file, updating project type, and so on), respectively. FileManager service and ProjectOperations are provided by the Process Manager and FileManager core modules of Roo, respectively. It is important to note that add-ons are different from core modules in Roo. The core modules provide vital features of the Spring Roo tool, like file system monitoring, registering commands with the Roo shell, and so on. Roo commands provided by add-ons are executed by Spring Roo users for code generation but Roo commands provided by core modules are primarily meant for accessing internal features of Spring Roo, like obtaining metadata, setting polling speed, and so on.

We saw earlier that the isInstallTagsCommandAvailable method of MysimpleOperations is invoked by the isInstallTagsCommandAvailable method of the MysimpleCommands class to check the availability of the web mvc install tags command. The following code shows the isInstallTagsCommandAvailable method of MysimpleOperationsImpl:

```
public boolean isInstallTagsCommandAvailable() {
    return
        projectOperations.isProjectAvailable() &&
        fileManager.exists(projectOperations.getProjectMetadata()
        .getPathResolver().getIdentifier(Path.SRC_MAIN_WEBAPP,
        "WEB-INF" + SEPARATOR + "tags"));
}
```

In the code, the isInstallTagsCommandAvailable method makes use of ProjectOperation services to check if a Roo project exists. The method also makes use of the FileManager service to check if a tags sub-directory exists in the SRC_MAIN_WEBAPP/WEB-INF directory of your Roo project. If a tags directory doesn't exist or you haven't yet created a Roo project, then the method returns false. This means the web mvc install tags Roo command is not available to the Roo shell if you haven't yet created a Roo project which contains a tags sub-directory inside the SRC_MAIN_WEBAPP/WEB-INF directory.

We saw earlier that the installTags method of MysimpleCommands invokes the installTags method of MysimpleOperations. The following code shows the installTags method as implemented by the MysimpleOperationsImpl class:

```
import org.springframework.roo.process.manager.MutableFile;
import org.springframework.roo.project.Path;
import org.springframework.roo.project.PathResolver;
import org.springframework.roo.project.ProjectOperations;
import org.springframework.roo.support.util.FileCopyUtils;
import org.springframework.roo.support.util.TemplateUtils;
...
public void installTags() {
    PathResolver pathResolver =
    projectOperations.getProjectMetadata().getPathResolver();

    createOrReplaceFile(..., "info.tagx");
    createOrReplaceFile(..., "show.tagx");
}

private void createOrReplaceFile(String path, String fileName)
{
    String targetFile = path + SEPARATOR + fileName;

    MutableFile mutableFile = fileManager.exists(targetFile) ?
    fileManager.updateFile(targetFile) :
    fileManager.createFile(targetFile);
    try {
    FileCopyUtils.copy(TemplateUtils.getTemplate(getClass(),
    fileName), mutableFile.getOutputStream());
    } catch (IOException e) {
    throw new IllegalStateException(e);
    }
}
```

In the code, the installTags method copies info.tagx and show.tagx files from the SRC_MAIN_RESOURCE/com/roo/addon/mysimple directory of the mysimple add-on to the SRC_MAIN_WEBAPP/WEB-INF/tags directory of your Roo project. The createOrReplace method is the method, which is used by the installTags method to copy the files. The following table describes the classes used by the installTags and createOrReplace methods for copying tag files:

| Class | Description |
|---|---|
| PathResolver | Used to locate files and directories in your Roo project. You can use the ProjectOperations service to obtain reference to PathResolver. |
| MutableFile | Represents a file in your Roo project, which you want to create, modify, or delete. You can use the FileManager service to obtain reference to the MutableFile instance. |
| FileCopyUtils | Utility class that provides methods for copying resources from the add-on to your Roo project. |
| TemplateUtils | Utility class that is used to resolve template files in the add-on project. We'll discuss templates in detail later in this recipe. |

Let's now look at the MysimplePropertyName enum type, which defines constants for the countryOfOrigin argument of web mvc install tags command.

## MysimplePropertyName enum type

The following code shows the MysimplePropertyName enum type, which defines constants for the countryOfOrigin argument value of the web mvc install tags command:

```
public enum MysimplePropertyName {
    AUSTRALIA("Australia"),
    UNITED_STATES("United States"),
    GERMANY("Germany"),
    NOT_SPECIFIED("None of your business!");

    private String propertyName;

    private MysimplePropertyName(String propertyName) {
        Assert.hasText(propertyName, "Property name required");
        this.propertyName = propertyName;
    }

    ...
}
```

In the code, constant AUSTRALIA is associated with value Australia, UNITED_STATES is associated with value United States, and so on. We saw earlier that the countryOfOrigin argument is of type MysimplePropertyName. We can only pass String type values for an argument from the Roo shell, so what we should specify as the value of the countryOfOrigin argument, and how it'll get converted to MysimplePropertyName. When you enter a partial value for the countryOfOrigin argument and press the Tab key, Roo internally refers to MysimplePropertyName to find a matching constant. For instance, if you enter au as the value of the countryOfOrigin argument, Roo attempts to find the constant that matches au in MysimplePropertyName and auto-completes the value. As the matching is case-insensitive, the value au of the countryOfOrigin argument is completed by the Roo shell as AUSTRALIA.

The following diagram summarizes how a simple add-on works:

![](img/b8c379a2ed921ead3aa2b01811dc29fa_398_0.png)

The figure shows that CommandMarker is an interface provided by Roo, and it is implemented by the MysimpleCommands class. The MysimpleCommands class invokes methods of the MysimpleOperationsImpl class to process the commands exposed by the mysimple add-on. The MysimpleCommands and MysimpleOperationsImpl classes use services provided by Roo to perform the desired functionality.

## There's more...

In this section we'll look at:

- How to locally deploy the mysimple add-on for testing Roo commands
- How tab-completion support is implemented for Roo commands using constants defined in an enum type and a Java class
- How the @CliAvailabilityIndicator annotation can be used for a method to define availability of multiple Roo commands exposed by the *Command class
- What templates are in add-ons and how they are typically used
- Plugins and dependency configuration in the pom.xml file of an add-on

## Deploying and running mysimple add-on

Once you have created an add-on, you may want to test its functionality, before making the add-on available to other developers. In this section, we'll look at how to locally deploy and test the mysimple add-on that we created using the `create addon simple` command.

To use the mysimple add-on, you need to convert it into an OSGi-compliant JAR bundle. To do so, execute `mvn clean install` from the directory which contains your mysimple add-on project, as shown here:

```
C:\roo-cookbook\ch07-simple-add-on> mvn clean install -Dgpg.passphrase=<thephrase>
```

Here, `<thephrase>` is the password phrase that you provide for signing add-ons using GnuPG (also referred to as GPG). Refer to the *Setting up GnuPG for add-on development* recipe for information on how to set up GnuPG and create a password phrase for signing add-ons.

Executing the `mvn clean install` command creates a `com.roo.addon.mysimple-0.1.0.BUILD-SNAPSHOT.jar` add-on OSGi bundle in the `target` directory of the mysimple add-on project. Now, let's look at how to use the mysimple add-on in a Roo project.

## Using the mysimple add-on in a Roo project

The following steps will demonstrate how to use an add-on:

1. Create a new directory `C:\roo-cookbook\ch07-addon-test` in your system and start the Roo shell from this directory.

2. Execute the `ch07_web_app.roo` script to create a `flight-app` Spring Web MVC project.

3. Execute the `osgi start` command to install and activate the mysimple add-on, as shown here:
```
roo> osgi start --url file:///C:/roo-cookbook/ch07-simple-add-on/target/com.roo.addon.mysimple-0.1.0.BUILD-SNAPSHOT.jar
```
Here the `url` argument specifies the location of the add-on OSGi bundle you want to install and activate.

The `osgi start` command installs the mysimple add-on. This command is also used to download and install Roo add-ons that are located on a website by specifying the `http://` or `https://` URL to the add-on JAR file as the value of the `url` argument.

4. To verify that the add-on was successfully installed, type `say` at the Roo shell and press the `Tab` key. Roo should autocomplete the command to `say hello`, as shown here:
`roo> say hello`
