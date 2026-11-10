# 25

# Documentum Deployment

In any well-defined software lifecycle, you develop the application on a development environment and get it QA'ed (or system tested) on an altogether separate test environment. By environment we mean a complete hardware and software setup consisting of a suite of servers and programs in place, necessary for the developed application to function properly. Subsequently, the system-tested application is released on staging and then eventually on production servers. While the environments keep on changing from development to system testing to production, what remains constant is a common deployment strategy. This strategy should be well planned, consisting of steps that involve minimal human intervention to ensure they can be performed repetitively for migrating our application across various environments.

In Documentum, a typical application would have the following components or entities that need to be migrated across the various environments:

+   - Docbase objects such as object types, lifecycles, workflows, alias sets, templates, presentation files, and such other objects that lay the foundation of the application
+   - Content existing in the Docbase created from templates or imported files, images, etc. in the Docbase
+   - Web Publisher code and customized files
+   - Any post-deployment scripts that need to be executed on the target environment (such as scripts for creating Site Publishing Configurations)

![](bbox=[0.28, 0.58, 0.71, 0.7])

Figure 25.1: Deployment from source to target environment

There are numerous deployment methodologies available that assist us in migrating objects and content from one environment to another. Each of these has its own share of advantages and disadvantages and should be chosen after a proper evaluation of your specific needs.