# There's more...

Another example of running Ant through Groovy is the creation of custom property files on the fly. This allows you to pass on information from one Jenkins job to another.

You can create property files through AntBuilder using the echo task. The following lines of code create a value.properties file with two lines, x=1 and y=2:

```
def ant = new AntBuilder()
ant.echo(message: "x=1\n", append: "false", file: "values.properties")
ant.echo(message: "y=2\n", append: "true", file: "values.properties")
```

The first echo command sets append to false, so that every time a build occurs, a new properties file is created. The second echo appends its message.

You can remove the second append attribute as the default value is set to true.