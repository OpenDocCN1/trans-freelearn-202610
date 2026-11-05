# Required parts of a plugin

A `coily` plugin closely resembles a Python package with some slight tweaks. This doesn't mean that a plugin is meant to be installed as a Python package. It is only a description of the folder structure.

Let's look at the layout of the `gen-cherrypy-app` plugin as an example.

![](bbox=[0.33, 0.26, 0.66, 0.36])

Some parts of this layout are required, and other parts are not. The top folder is the name of the plugin.

+   - A plugin requires a `__init__.py` file inside the top directory.
+   - `__init__.py` must include a `__description__` variable. This description is shown when we run the `coily --help` command.
+   - `__init__.py` must include a command function, which is either a `create` or `apply` function. `create` is used when the plugin needs one argument from the user. `apply` is used when no argument is needed from the user.

Let's look at how `gen-cherrypy-app` meets each of these requirements.

+   1. We can already see from the diagram that the top level folder has the same name as our plugin.
+   2. Inside `__init__.py`, we can see the following help message defined.
+      `__description__ = "plugin to create skeleton CherryPy applications"`
+   3. `gen-cherrypy-app` is used to create a skeleton application. It needs the user to supply the name of the application it will create. Again, looking inside `__init__.py`, the following method signature can be found.
+      `def create(plugin_path, name)`
+   4. `plugin_path` is an argument provided to `gen-cherrypy-app` by `coily`, which points at the base directory of `gen-cherrypy-app`. This argument is also provided for plug-ins that use the `apply` command function.
+   5. `name` is the name of the application provided by the user.