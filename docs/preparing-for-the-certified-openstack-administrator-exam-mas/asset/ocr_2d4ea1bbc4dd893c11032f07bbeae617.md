# Project scope

Project scope is the most fundamental aspect of any OpenStack environment. If you are attempting an objective on the Horizon dashboard, don't forget to always scope yourself to the proper project from the top dropdown. Refer to Figure 11.1.

If for any reason you do not see a project from the top dropdown, you should jump to the CLI.

![](bbox=[0.27, 0.25, 0.76, 0.66])

Figure 11.1: Scoping to a project on the Horizon dashboard

If you are on the CLI, the easiest way to scope yourself to a new project is using the method presented in the previous chapters of this book:

```
$ cp openrc openrc-<project-name>
```

After making a copy of the original openrc file, use the editor of your choice to fill in the proper variables for project and tenant:

```
$ nano openrc-<project-name>
```