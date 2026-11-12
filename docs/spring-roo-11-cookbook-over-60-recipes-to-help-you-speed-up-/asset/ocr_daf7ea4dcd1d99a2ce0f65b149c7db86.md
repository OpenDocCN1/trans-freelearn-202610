# The pom.xml file

The pom.xml file of an add-on created via the addon create simple command contains the following configurations:

+   - The core Spring Roo modules on which a simple add-on depends is configured in the pom.xml file. If your add-on makes use of other add-ons, then you'll need to configure it in the pom.xml file.
+   - By default Google Code is configured as the SCM (Software Configuration Management) repository for the add-on.
+   - The Maven assembly plugin is configured for packaging the add-on. You can execute perform assembly Roo command or assembly:single goal of the assembly plugin to package the add-on as a ZIP file. The assembly description, assembly.xml, is located in the src/main/assembly folder of add-on.
+   - The Maven release plugin is configured for releasing the add-on. Once you are done with local testing of your add-on, you can release the add-on to Google Code (or the SCM you configured in the pom.xml file) by executing the mvn release:prepare release:perform Maven command.
+   - The Maven GPG plugin is configured to sign add-on project artifacts using GnuPG.
+   - The Maven bundle plugin is configured to package an add-on as an OSGi compliant bundle. You'll find that the <packaging> element's value is specified as bundle in the pom.xml file of add-on, which means that the add-on is packaged as an OSGi bundle.

# OSGi commands for troubleshooting

Once you have deployed an add-on, you can check if it was successfully installed or not by using the following OSGi commands from the Roo shell:

+   - osgi ps: lists the OSGi bundles and their status. If you have successfully installed the mysimple add-on, then executing the osgi ps command should show the mysimple add-on as active, as shown here:
+     [Active] [1] Mysimple addon (0.1.0.BUILD-SNAPSHOT)
+   - osgi log: shows the OSGi container logs. If your add-on fails to install successfully, you can refer to the container logs to troubleshoot installation issues.
+   - osgi scr list: lists services and components registered with the OSGi container. If you have successfully installed the mysimple add-on, then executing the osgi scr list command should show commands and operation types, as shown here:
+     [181] [active] com.roo.addon.mysimple.MysimpleOperationsImpl
+     [180] [active] com.roo.addon.mysimple.MysimpleCommands