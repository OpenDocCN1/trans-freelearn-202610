
The `class_name` field is the alias for the field name of the `classes` table. We have defined this alias in `INNER JOIN` made through the `Select` class.

Although we handle the data in memory as objects, we need to convert the data into arrays to persist them in the table. So, in the `Student->toArray` method, we replace the object with the primary key value:

```
public function toArray()
{
    $attributes = get_object_vars($this);
    $attributes['class_code'] = $attributes['schoolClass']
        ->code;
    unset($attributes['schoolClass']);
    return $attributes;
}
```

To insert a new student, we need to associate it with a school class. This requires a list of school classes in the edition form for students. In the next part of the code, we can see the part of the `edit.php` file of `view/student` that creates this list:

```
<select name="class_code">
<?php
$schoolClasses = SchoolClassTable::getInstance()->getAll();
foreach($schoolClasses as $schoolClass) :
?>
<option value="<?=$schoolClass->code?>"
    <?=($schoolClass->code == $Student->schoolClass->code ?
    'selected="selected"' : '')?>>
    <?=$schoolClass->name?></option>
<?php
endforeach;
?>
</select>
```

These are the main differences between school classes and student registrations. The `school2` project is fully available at `https://github.com/PacktPublishing/PHP-Web-Development-with-Laminas/tree/main/chapter04/school2`. There is a `StudentTest` test case for student registration and a report of the code coverage. Try the complete application and compare the code of both registrations. In Eclipse, you can compare two files by selecting them and pressing the right mouse button, following which you select **Compare with/Each Other**.

Now you know how to abstract a database connection and how to create ORM using `laminas-db`.

In the next section, we will make a new implementation of the school classes and student registrations, but this time we will use the `laminas-mvc` component as a container for our application.

## Using Laminas as a container

When you use Laminas as a library, your code calls the Laminas components. It’s what we did with the `school1` and `school2` projects. Now, we will use Laminas as a framework and this framework will call your code. Previously, Laminas was contained in our applications. Now, we will create an application that will be contained in the framework. In fact, we will rewrite our code according to the MVC structure of Laminas.

As this is a preparation for a more complex and complete application, we will show only the implementation for the listing of the school classes, but do not worry, because the complete code is available at the end of the section. But try to implement the code along with the explanation before copying it from the repository, so you can compare your expectations with the provided code.

Like we did before, we need to prepare for the implementation by creating a new project. Let us create it!

## Creating the school3 project

To create the `school3` project, follow these steps:

- 1. Create the project.

  Do you remember that we have configured an item in the Eclipse toolbar that creates Laminas projects? We did it in the previous chapter. Well, you can use it to create the `school3` project.

  Open the project using the `File | New | PHP Project` menu option.

- 2. Add PHPUnit to the `school3` project as a development dependency.

- 3. Don’t forget to save the `composer.json` file and click on the **Update dependencies** button.

> **Allow plugins**
> 
> When you are installing a component after creating the project from the skeleton, you might be asked **Do you trust ‘laminas/laminas-component-installer’ to execute code and wish to enable it now?** and **Do you trust ‘laminas/laminas-skeleton-installer’ to execute code and wish to enable it now?**. These messages are related to Composer plugins. You can answer y.

- 4. After installing PHPUnit, select the `school3` project with the right mouse button and access the Run As | PHPUnit Test item. You will see the following message on the Console tab:

```
Fatal error: Uncaught Error: Class "Laminas\\Test\\PHPUnit\\Controller\\AbstractHttpControllerTestCase" not found
in /opt/lampp/htdocs/school3/module/Application/test/
Controller/IndexControllerTest.php:11
```

This error occurs because the Laminas skeleton project depends on the `laminas-test` component, an extension for PHPUnit that allows us to test the Laminas controllers. As you have installed PHPUnit in the `composer.json` editor, search for the `laminas/laminas-test` package and install it. We learned how to install a Composer package in *Chapter 3, Using Laminas as a Library with Test-Driven Development*.

- 5. Now you can run the test successfully. But what test is running? The `IndexControllerTest` class in the `module/Application/test/Controller` folder.

> **Errors when testing Laminas MVC in PHP 7**
> 
> Maybe you remember that in *Chapter 2, Setting Up the Environment for Our E-Commerce Application*, we showed PHP requirements for `laminas-mvc`. The `composer.json` file allows PHP 7. However, if you run `IndexControllerTest` under PHP 7, there will be an error for the `testIndexActionViewModelTemplateRenderedWithinLayout` test method. In fact, the error occurs because the `$this->assertQuery` method depends on union types, available only from PHP 8. You can replace `$this->assertQuery` with other assertion methods and keep using `laminas-test` under PHP 7, but it is better to upgrade PHP to version 8. But if you are required to use PHP 7 for some reason, then remove the `testIndexActionViewModelTemplateRenderedWithinLayout` method.

Now that we have run the test, we will create a new module.

## Creating the School module

The Laminas MVC application is organized as a group of modules. A module in Laminas is the biggest unit of reuse. A module is self-contained. It has its own configuration and its own MVC implementation. To implement our school class registration with `laminas-mvc`, we will create another module, named `School`. The Laminas modules are located in the `modules` folder. There is a default module named `Application`. The `Application` module contains the sample home page that you viewed in the previous chapter.

We will use the `Application` module as the model for a new module:

- 1. Copy the `Application` folder in the `modules` folder as `School` in the same `modules` folder.

The namespace of a module in Laminas is the name of the module. So, the namespace of the Application module is Application and, therefore, the namespace of the School module must be School. We will change the namespace of each file as we rebuild our implementation. Let's start with the minimal code for the module.

- 2. Register the module in the modules.config.php file.

The modules.config.php file is in the config folder in the project root. This file contains the list of enabled modules as a PHP array. Modules outside of this list are ignored, as if they don't exist. This makes it easy to disable a module by removing it from modules.config.php. You can see that Application is the last item. It is not random. The last module is what imposes the layout. The layout is the arrangement of visual elements. For a web application, the layout is defined with Cascade Style Sheets (CSS). Our new module, School, does not have a layout, so it can't be the last.

- 3. Add School as an item before Application.

A module listed in modules.config.php will be searched for. But the modules will only be found if they have autoloading configurations. You should remember that we have configured autoloading via Eclipse for the school1 project. You can use the same feature to configure composer.json of school3.

- 4. Select the Autoload tab in the Composer code editor. You will see that there is already a PSR-4 configuration for the Application module. You must add a new autoloading configuration with these parameters:

  - * **Namespace:** `School\\
  - * **Paths:** `module/School/src/`

We have to add a PSR-4 autoloading configuration for test classes too.

- 5. Select the Autoload-dev tab. Add a configuration with these parameters:

  - * **Namespace:** `SchoolTest\\
  - * **Paths:** `module/School/test/`

- 6. Save the composer.json file and click on the **Update dependencies (including require-dev)** button.

> **Editing composer.json directly**
> 
> Every change we are making with specific forms can also be made directly. You can click on the **composer.json** tab at the bottom of the composer.json view. The editor will open the file as a text file. You can see the changes made by other tabs and you can also make changes directly.
