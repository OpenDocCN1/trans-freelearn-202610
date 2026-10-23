As we can see in the preceding output, we have an audit trail of who created the user from the preceding command as well as what the project was and the resource ID. This data and all of the data from the other services combined would provide a 7 W's audit trail for this user's transactions as well as any other transactions in this cloud. Using either open source tools such as Ansible, Puppet, or Chef to create query automation for Ceilometer, log files or having Ceilometer feed this event data to something like IBM's QRadar http://www-03.ibm.com/software/products/en/qradar which is, according to IBM:

> "[A] security intelligence platform that combines traditional security information and event management (SIEM) and log management capabilities with network behavior anomaly detection(NBAD), vulnerability assessment and management, risk analysis and simulation and forensic data inspection. It consumes events, flows, asset and vulnerability information and network topology by integrating with other products/applications/services/assets/endpoints in a client’s environment (on-premises or in the cloud), enabling users to view, analyze, understand and report on everything going on in their environment from many different angles and perspectives."

QRadar integrates with OpenStack as a consumer of Ceilometer data. It then takes the CADF formatted data in the platform to do the analysis.

# Log aggregation and analysis

As we saw in the previous section, we have an audit trail of who created the user from the demonstrated command as well as what the project was and the resource ID. This data and all of the data from the other services combined will provide a 7 W's audit trail for this user's transactions as well as any other transactions in this cloud. However, since we now have audit data, we can start looking for security attacks. Some of these attacks may be as follows:

+   - Real-time security alerts for brute force attacks OpenStack
+   - Detecting malicious or unauthorized access to virtual machines, volumes, or images
+   - Accidental or intentional cloud service outages
+   - Compliance reporting for all user activity in OpenStack