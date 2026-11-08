While designing in Artisteer, you can't make a particular module different from others, as changes spread into the others blocks. That's the reason we need a solution.

We are working on the `specialModule.artx` project with content. Open it and export it to Joomla!. Your site should look like the following screenshot:

![](bbox=[0.17, 0.23, 0.83, 0.6])

Sample site with two identical modules on the left-hand and right-hand columns

The site contains only one page, has a three-column layout, and two identical modules on the left-hand and right-hand columns. They were imported as two separate custom HTML modules, but both were created in Artisteer. Our task is to distinguish the second module, without modifying its HTML, as follows:

+   1. In Joomla!'s administrative panel, navigate to **Extensions | Module Manager**. You will go to the **Modules** screen in **Module Manager**, which will present all installed modules. Find and click on the **Second Block** module to go to its settings.
+   2. In **Module Manager: Module Custom HTML** (**Second Block** module settings), expand the **Advanced Options** area and find the **Module Class Suffix** field. In this field, we are going to type a suitable suffix.