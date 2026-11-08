# Adding the nightly build libraries

If you'd run the build of the project now, the Apache Maven client would try to download all the Pentaho dependencies in their newest version, requested from the pom.xml file (in our example, 8.1-SNAPSHOT). Considering that the newest version would not be published yet in the Pentaho's Sonatype Nexus Repository, an error would occur and your build would fail.

To solve this issue, there is a workaround, which is adding the Pentaho newest dependencies into your local Apache Maven repository. In brief, when Apache Maven looks for a dependency, it first looks into the local repository (in the .m2/repository folder), and if not found there, it tries to download it from the remote repository. By adding the newest Pentaho dependencies into your local Apache Maven repository, you will skip the need to go to the remote repository, avoiding the failure.

> *Be aware that this is a workaround to enable you to use the latest (and possibly unstable) Pentaho Reporting nightly build. Adding libraries to a local Maven repository is not a best practice and should be limited as much as possible.*

To install a library or an artifact in general to a local Apache Maven repository, there is a command similar to the following one:

```
mvn
  install:install-file
  -Dfile=path/to/artifact/name.jar
  -DgroupId=...
  -DartifactId=...
  -Dversion=...
  -Dpackaging=jar
```

By using this command for each Pentaho dependency, you can update your local Apache Maven repository with the nightly build. The following is the command list to run, to add all the requested dependencies, to build with success all the examples described in this book: **mvn install:install-file -Dfile=./report-designer/lib/classic-core-8.1-SNAPSHOT.jar -DgroupId=pentaho-reporting-engine -DartifactId=pentaho-reporting-engine-classic-core -Dversion=8.1-SNAPSHOT -Dpackaging=jar**