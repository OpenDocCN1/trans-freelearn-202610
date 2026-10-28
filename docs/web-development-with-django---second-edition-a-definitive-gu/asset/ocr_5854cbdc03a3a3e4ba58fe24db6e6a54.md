Django creates the prefix directories inside STATIC_ROOT. Because of this, the paths can be kept consistent, even when using a web server and not routing the URL lookup through Django.

Next, we’ll look at a management command that’s useful for troubleshooting when static files aren’t being loaded as you expect: findstatic.

# The findstatic command

The staticfiles application also provides one more management command: findstatic. This command allows you to enter the relative path to a static file (the same as what would be used inside a static template tag) and Django will tell you where that file was located. It can also be used in verbose mode to output the directories it is searching through.

> **Note**
> You may not be familiar with the concept of verbosity, or verbose mode. Having a higher verbosity (or simply turning on verbose mode) will cause a command to generate more output. Many command-line applications can be executed with more or less verbosity. This can be helpful when you’re trying to debug programs you are using. To see an example of verbose mode in action, you can try running the Python shell in verbose mode. Enter python -v (instead of just python) and press Enter. Python will start in verbose mode, which prints out the path of every file it imports.

This command is mostly useful for debugging/troubleshooting purposes. If the wrong file is loading, or a particular file can’t be found, you can use this command to try to find out why. The command will display which file on disk is being loaded for a specific path, or let you know that the file can’t be found and what directories were searched.

This can help solve issues where multiple files have the same name, and the precedence is not how you expect. You may also see that Django is not searching in a directory you expect for the file, in which case the static directory might need to be added to the STATICFILES_DIRS setting.

In the next exercise, you will execute the findstatic management command so that you are familiar with what the outputs are for good (file found correctly) and bad (file missing) scenarios.