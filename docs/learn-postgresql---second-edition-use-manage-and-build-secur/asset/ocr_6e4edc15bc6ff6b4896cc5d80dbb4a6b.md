```
load      load a distribution's extensions into a database
mirror    return information about the available mirrors
search    search in the available extensions
uninstall
          remove a distribution from the system
unload    unload a distribution's extensions from a database
```

Usually, you will use the following subset of commands:

+   - search to search for distributions using keywords
+   - info to have a closer look at an extension
+   - download to download (but not install) an extension
+   - install to download and install an extension in the cluster
+   - load to execute CREATE EXTENSION against a specific database
+   - unload to execute DROP EXTENSION against a specific database
+   - uninstall to remove an extension from a cluster

The smallest set of commands you will probably use are search, install, and uninstall.

For every command, you can get more detailed help if you specify the command as an argument to the help command. For example, to get more information about the search command, you can do the following:

```
$ pgxn help search
usage: pgxn search [--help] [--mirror URL] [--verbose] [--yes]
                    [--docs | --dist | --ext]
                    TERM [TERM ...]

search in the available extensions

positional arguments:
  TERM        a string to search

optional arguments:
  --help      show this help message and exit
  --docs      search in documentation [default]
  --dist      search in distributions
  --ext       search in extensions
```