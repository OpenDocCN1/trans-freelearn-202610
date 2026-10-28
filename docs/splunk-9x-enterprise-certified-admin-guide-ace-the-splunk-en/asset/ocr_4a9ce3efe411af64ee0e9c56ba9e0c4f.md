10. If you have an app that is specific to Linux forwarders and you need to block it from Windows forwarders, which settings should you choose in serverclass.conf?

+   A. Allow/deny settings.
+   B. Whitelist/blacklist.
+   C. Whitelist for Linux hosts and blacklist for Windows hosts on the same server class.
+   D. No action – forwarders are intelligent enough to choose the right apps.

I hope you were able to recollect the topics that we went through while going through these questions. We’ll review the answers in the next section.

# Reviewing answers

Here are the answers to this chapter’s questions:

+   1. False – the UF cannot store, search, and index the data. It doesn’t have any user interface to configure either.
+   2. Options A and C are the correct answers. By default, the UF comes with an embedded license, so no extra license is required. The UF can parse structured data by setting INDEXED_EXTRACTIONS in props.conf locally. The UF will resume from the file’s last read position by making use of the fishbucket index, which contains seek pointers.
+   3. Options A and B are the correct answers. By default, the DS is enabled on all Splunk instances. Apps are stored in $SPLUNK_HOME/etc/deployment-apps. The DS and forwarders always need connectivity and they communicate at defined intervals.
+   4. Option B is correct. serverclass.conf is the file that contains the app mapping to forwarder clients and other settings such as whitelist, blacklist, restartSplunkd, and others.
+   5. Option C is correct. deploymentclient.conf is the file that contains the DS host and port details to establish a connection.
+   6. Options B, C, and D are the correct answers.
+   7. Options C and D are the correct answers. Both the UF and HF can be managed through the DS. The restartSplunkd setting helps reboot the remote forwarder once apps have been deployed.
+   8. Options B, C, and D are the correct answers. The internal logs of the forwarder and DS are used inside the MC app dashboards.
+   9. Option B is correct. 60 seconds is the default phone home interval.
+   10. Option C is correct. Whitelist and blacklist are the two useful settings in such cases.