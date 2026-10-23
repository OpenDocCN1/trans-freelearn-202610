On the panel for configuring user token security settings, leave the box unchecked for Create maximum strength keys. That will be fine for this demonstration system. For production, maximum strength keys are a good idea, but you may have to configure the WAS Java environment with a policy file download. We're avoiding that complication for now, but you can read about user tokens and related cryptography settings in the XT documentation.

Choose EAR file deployment (not WAR file deployment). Work through the remaining installer panels and complete the XT software installation. When you have finished, the XT software will have been installed, but the XT EAR file is not yet deployed to the J2EE application server.

XT writes log files to a directory within the installation tree. So that the running WAS process can create files in the directory, perform the following command, which changes the ownership of the XT logging directory to ecmuser and the group to ecmgroup:

```
# chown ecmuser:ecmgroup /opt/ibm/FileNet/WebClient/LogFiles
```

# CE and PE client software

XT communicates with both the CE server and the PE server. Like any other application, XT needs access to the client-side software provided by those servers.

If you are installing both AE and XT, you can do these steps after you have run the installers for both AE and XT, in which case the client updates will be done for both. It's no different from doing them separately for AE and XT; it just saves you a little trouble.

# CE client

The CE client software and configuration files must be installed into the XT environment. This is similar to the step we did earlier for the PE server. In this case, the CE client installer name is P8CE-CLIENT-4.5.1-LINUX.BIN. The presence of the XT installation should be automatically detected if you used the default path in the XT installer. The rest of the installation should be uneventful.

Since you are going to be installing the PE client software immediately after this, you can save a little time by skipping the creation of the EAR file right now. On the Create Deployment File panel, select Create the file later.