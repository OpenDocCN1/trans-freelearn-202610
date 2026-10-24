The deployment of an OSGi application is called provisioning as it gathers the following:

+   - The installation of a set of bundles, including transitive bundles
+   - The installation of a set of configurations required by these bundles

# OBR

OSGi Bundle Repository (OBR) can be the first option to be considered in order to solve this problem. Apache Karaf can connect to the OBR server. The OBR server stores all the metadata for all the bundles, which includes the capabilities, packages, and services provided by a bundle and the requirements, packages, and services needed by a bundle. When you install a bundle via OBR, the OBR server checks the requirement of the installed bundle and finds the bundles that provide the capabilities matching the requirements. The OBR server can automatically install the bundles required for the first one.

# Apache Karaf Features

As a lightweight and standalone OSGi container, Apache Karaf proposes another way to provision applications. Apache Karaf Features is the default provisioning solution in Apache Karaf.

Karaf Features describes an application, declaring the following aspects:

+   - The bundles in the application
+   - Other Karaf features required by the application
+   - Configurations required by the application

A Karaf Feature repository is a simple XML file describing a set of Karaf features.
A Karaf Feature repository looks like the following:

```
<features name="my-features">

    <feature name="core-feature" version="1.0">
        <bundle>mvn:bundleA_groupId/bundleA_artifactId/bundleA_version</bundle>
        <bundle>file://path/to/bundleB</bundle>
        <bundle>http://path/to/bundleC</bundle>
    </feature>

    <feature name="extend-feature" version="1.0">
        <feature version="[1,2)">core-feature</feature>

```