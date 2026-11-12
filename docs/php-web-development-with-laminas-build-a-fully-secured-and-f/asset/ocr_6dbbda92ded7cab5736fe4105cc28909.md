```
->addInput('operator', [
    new AllowList(['list' => ['-','*','/']])
])
->addInput('factor', [new ToFloat()]);

return $inputFilter;
}
```

Observe that we call the addInput method several times, but we need to use the $inputFilter variable only once. It is because the addInput method returns the reference to the InputFilter instance.

Well, we have reduced the Discount->getInputFilter method by several lines. In the same way, we can reduce the Product->getInputFilter method by several lines. After refactoring the source code, we need to run tests to check whether everything keeps working. Before running the tests, we need to recreate the vendor directory by running the composer install command.

After checking that all code passes the tests, and after removing the vendor directory again, we can run phpcpd again. This time, after refactoring, we will see output that looks like this:

```
$ phpcpd whatstore/
phpcpd 6.0.3 by Sebastian Bergmann.

No clones found.

Time: 00:00.563, Memory: 4.00 MB
```

As you can see, the refactoring has eliminated the duplicated code from the whatstore project. The phpcpd tool is really useful to help us find duplicated code with TDD.

> **PHP Mess Detector**
> 
> There is another interesting CLI tool for PHP named the **PHP Mess Detector (PHPMD)**. This tool looks for several potential problems within PHP source code, such as possible bugs, suboptimal code, overcomplicated expressions, and unused parameters, methods, and properties. It is recommended that you use PHPMD combined with your IDE. You can download PHPMD at https://phpmd.org.

In the next section, we will do some more refactoring, this time to remove unnecessary configurations.