10. The configuration program installs SCSDocApp on the selected Docbase, configuring the Docbase to be used with SCS services. The following objects are installed as part of SCSDocApp:

+   - `dm_webc_config` object type
+   - `dm_webc_target` object type
+   - `webc_lock` object type
+   - `dm_SCSLogPurgeJob` job
+   - `dm_SCSLogPurge` method
+   - `PostInstall` procedure

Click Finish once the DocApp has been installed.

11. Restart the service Documentum SCS_source_<port number> (for example: Documentum SCS_source_6677 if SCS Source is configured on default port 6677).

This completes the installation of SCS Source 5.2.5. We can now safely go ahead and install service pack 2 over this installation.

> Every time a new Docbase has been created, we need to run the SCS Source configuration program (config.exe) from %DM_HOME%\webcache\install on the Content Server host. Failing to do so will result in errors during publishing from the Docbase in question.

# 7.1.3 Installing SCS Source 5.2.5 SP2

+   1. Extract Site_caching_services_5.2.5_SP2_all.zip on the Content Server host and copy the webcache.jar file to the following location on the Content Server host: `%DM_HOME%\webcache\tomcat\webapps\webcache\WEB-INF\lib`
+   
+      Example: A typical location on Windows could be:
+      `c:\documentum\product\5.2\webcache\tomcat\webapps\webcache\WEB-INF\lib`
+   
+   2. Restart the SCS Source service.

That's it! We are done with upgrading SCS Source to SP2 and can now install SCS Target 5.2.5 and then upgrade it with service pack 2.

> You can monitor the state of SCS Source to find out its product version and other information such as total uptime, total incremental publishes, etc by viewing the following URL from your browser: http://<Content Server host>:6677/webcache/state

If the SCS Source service is not running, the following error message is seen when trying to publish using Site Caching Services from a Docbase:

Error occurred while publishing the Site Publishing Configuration
[DM_METHOD_E_HTTP_COMMUNICATION]error: "Failed to obtain socket for host:localhost, at port:6677"