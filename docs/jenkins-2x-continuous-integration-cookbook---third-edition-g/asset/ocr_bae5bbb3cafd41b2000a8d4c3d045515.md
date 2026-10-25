![](bbox=[0.117, 0.062, 0.884, 0.383])

2. You can use wildcards, such as `module/dist/**/*.zip`. The artifact archiver uses Ant `org.apache.tools.ant.DirectoryScanner`, which excludes the following patterns by default: `**/%*, **/.git/**, **/.bzr, **/hg/**, **/.bzrignore, **/.git, **/SCCS/**, **/hg, **/#*`