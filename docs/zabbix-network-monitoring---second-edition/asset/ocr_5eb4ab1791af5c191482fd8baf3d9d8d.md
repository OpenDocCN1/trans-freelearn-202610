> It might be a good idea to use trigger functions such as avg() or max() to avoid trigger flapping, as discussed in Chapter 6, Detecting Problems with Triggers.

This would alert on any filesystem having less than 20% free disk space or being above 80% utilization. We could rewrite it to use the user macro as the threshold value:

```
{C_Template_Linux:vfs.fs.size[{#FSNAME},free].last()}<{$FS_FREE_THRESHOLD}
```

This would allow us to customize the threshold per host but not per filesystem. Expanding on this, we would instruct the LLD rule to put the discovered filesystem as the macro context, like this:

```
{C_Template_Linux:vfs.fs.size[{#FSNAME},free].last()}<{$FS_FREE_THRESHOLD:{#FSNAME}}
```

As the LLD prototypes are processed, the LLD macros are replaced with the discovered values in created items. The trigger for the root filesystem that would be created on the host would look like this:

```
{A test host:vfs.fs.size[{#FSNAME},free].last()}<{$FS_FREE_THRESHOLD:/}
```

The trigger for the /home filesystem would look like this:

```
{A test host:vfs.fs.size[{#FSNAME},free].last()}<{$FS_FREE_THRESHOLD:/home}
```

When Zabbix evaluates this trigger, it will first look for a macro with this context value on the host. If that is not found, it will look for this macro with this context in the linked templates. If it's not found there, it will look for a global macro with such a context. If it's still not found, it will revert to the macro without the context and evaluate that as a normal user macro. This means that we don't have to define user macros with all possible context values—only the ones where we want to modify the behavior. If there's a filesystem for which a specific user macro is not available, there's always the host, template, or global macro to fall back to.