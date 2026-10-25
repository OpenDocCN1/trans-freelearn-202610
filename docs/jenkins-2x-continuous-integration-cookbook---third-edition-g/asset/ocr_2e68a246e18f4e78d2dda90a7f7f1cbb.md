# Introduction

This chapter reviews the relationship between Jenkins, Ant, Gradle, and Maven builds, and there is also a small amount of scripting with Groovy and Ant.

Jenkins is the master of flexibility. It works well across multiple platforms and technologies. Jenkins has an intuitive interface with clear configuration settings. This is great for getting the job done. However, it is also important that you clearly define the boundaries between Jenkins plugins and Maven build files. A lack of separation will make you unnecessarily dependent on Jenkins. If you know that you will always run your builds through Jenkins, then you can afford to place some of the core work in Jenkins plugins, gaining interesting extra functionality.

Jenkins is technology agnostic and can glue together project technologies across the organization, development teams, and software position in their life cycle. Jenkins lets you run scripting languages of choice, and makes it easy to pull in source code using Git, Subversion, CVS, and a number of other version control systems. If Jenkins is not compatible, developers with a little practice can write their own integration.

In this book, you will see both Subversion and Git projects mentioned. This represents a realistic mix. Many consider Git more versatile than Subversion. Feel free to use Git as your repository of choice for the examples in this book. Designed in from the start, Jenkins makes it easy for you to choose between the different revision control systems.

If you look at the relative use of Git and Subversion for a representative collection from Ohoh in early 2014, for Git there were 247,103 repositories (37 percent of the total) and Subversion had 324,895 repositories (48 percent of the total).

Typical enterprises lag behind when using the most modern offerings because of their resistance to changing their working processes. Therefore, expect this category of businesses to have a higher percentage of Subversion repositories compared to smaller organizations.