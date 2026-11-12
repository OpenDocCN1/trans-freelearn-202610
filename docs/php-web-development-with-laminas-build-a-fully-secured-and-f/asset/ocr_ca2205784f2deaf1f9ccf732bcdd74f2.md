+   3. The Laminas application skeleton, which we have used since _Chapter 2, Setting Up the Environment for Our E-Commerce Application_, provides a template file for PHPUnit, `phpunit.xml.dist`. Copy it to `phpunit.xml`. You can see that the test suite is configured for running only the tests of the Application module:

```
<testsuite name="Laminas MVC Skeleton Test Suite">
    <directory>./module/Application/test</directory>
</testsuite>
```

+   4. Change the preceding code snippet to run the tests for any module:

```
<testsuite name="Laminas MVC Skeleton Test Suite">
    <directory>./module/*/test</directory>
</testsuite>
```

Great! We can take the first step of TDD, the red step, when we have a test that fails.

# Running the tests

Follow these steps to run the tests:

+   1. Select the `school3` project and click on the **Run As | PHPUnit Unit Test** item.

Of course, the test will fail because `SchoolClassController` doesn't exist.

+   2. Create `SchoolClassController` to manage the generation of the school classes listing. The file of this class must be created in the module/`SchoolClass`/`src/Controller` folder. In fact, all the classes of a module in Laminas stay in the `src` folder and all the controller classes stay in the `src/Controller` folder. This class needs two methods: a constructor in which we inject the Table Data Gateway and a method for invoking the table data gateway:

# SchoolClassController.php

```
<?php
declare(strict_types = 1);
namespace School\Controller;

use School\Model\SchoolClassTable;
use Laminas\Mvc\Controller\AbstractActionController;
use Laminas\View\Model\ViewModel;

class SchoolClassController extends
```