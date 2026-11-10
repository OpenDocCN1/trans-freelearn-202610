By choosing a type of generated code or comment and clicking on **Edit**, you will be able to add code and variables to the template. You can also choose to add comments on newly generated methods and types automatically.

The code template settings edited in this window are project specific. The settings are saved in a metadata file inside the project folder, so when a project is imported, the coding style settings are automatically set. This allows you to enforce the project's coding style among all the project's developers. If you want to edit the default code templates for the whole workspace, open the **Preferences** window and navigate to **Java | Code Style | Code Templates**. Projects that don't contain any specific setting for templates will use these settings.

# Refactoring

Code refactoring can be a real pain. As your project grows, adding a parameter to a method or renaming it can turn into a nightmare. Eclipse has some refactoring tools that make such tasks a walk in the park.

# Renaming variables

Renaming variables manually can be tedious and time consuming. You could resort to find/replace to make it a little less painful. But since the variable name might be matched in other places of the code, you have to go through each match of the variable name and verify if it has to be modified (try renaming a variable named x in a big enough class and you'll understand). If the variable is public, you would also have to scan the whole project for references of this variable.

Luckily, Eclipse does this for you. Just right-click on any occurrence of the variable in the code and navigate to **Refactor | Rename**. Type the new variable's name and hit *Enter*.

You can see that even if the name's variable occurs in other parts of the code, only the text that's within the variable's name is modified. Eclipse is only able to do that because it compiles the code and uses information from this process to know which ones are references of the variable in the code. Because it relies on the compiled code, it's not possible to rename a variable inside a class with compiling errors. Eclipse also won't allow you to change a variable's name to one that already exists in the same context. Renaming also works with other elements, such as classes, methods, and interfaces.