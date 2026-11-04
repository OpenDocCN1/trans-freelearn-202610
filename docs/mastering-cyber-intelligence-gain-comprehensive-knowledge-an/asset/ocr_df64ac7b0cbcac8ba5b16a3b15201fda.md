# CTI data storing requirements

Collected data and intelligence must be stored in the organization's environment to be accessible when needed. Intelligence is for everyone; hence, any security function should quickly use the data or information. When storing intelligence data, three metrics need to be considered:

+   - **Accessibility**: Security personnel, CTI analysts, and relevant stakeholders must be able to access the data or information at any time. The incident response team can use the data to detect, analyze, and remediate cyberattacks. The security operation team can access the data to perform system monitoring. The CTI team can still access the data for assessments and make informed decisions.
+   - **Availability**: Collected data must be stored so that it is available when needed. The organization must ensure that the storage infrastructure (hardware and software) is properly maintained and that all applications are functioning well and up to date. In worst-case scenarios, the storing infrastructure must be recovered promptly to avoid data loss.
+   - **Retrieval speed**: Intelligence data and information volume can be high. It falls under the big data category. Hence, the storage infrastructure must be designed to be queried quickly. Scanning through billions of indicator records can be annoying if the backend is not designed correctly.

These storage requirements are essential for reproducing and consuming intelligence within and outside the organization. When selecting feeds, the CTI team must evaluate the complexity of storing the data.

# Intelligence data storing strategies

Companies store intelligence data and information in two different ways: *organization-specific storage infrastructure* and *threat intelligence platforms*. Large companies can combine both methods to ensure data *replication* and *archiving*. (This is one of the best ways to maintain high availability. In the case of a failure, one system is used as a backup for the other.)