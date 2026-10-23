+   * For new assets, are the application and system log files integrated into the SIEM solution for auditing and monitoring?

+   * Have any discrepancies been discovered by SIEM or other asset management solutions?

During this review, the Technology Owners should also verify that each asset has been configured with a consistent time and date. NTP should be enabled on all devices that support it, and a reliable NTP source should be present in the industrial network.

# Reviewing and reporting known OT security risks

The Technology Owners are the most knowledgeable about the risks that exist within their areas of responsibility. It is prudent to review those risks during the periodic incident response preparation process to avoid becoming complacent with the existing risk level.

Some of the questions to be considered during that review include the following:

+   * Which OT assets are running an obsolete operating system?
+       - What is the mitigation and replacement plan for those assets?

+   * Which OT assets have and have not had at least one security patch applied in the past year?
+       - Are there OT assets with known vulnerabilities that have not been patched?

+   * Which OT assets are running unsupported hardware or software?
+       - What is the mitigation and replacement plan for those assets?

+   * What risks have been identified during MoC processes?

Findings should be reported to the OT Administrator.

# Incident simulation exercise

Periodically performing exercises that simulate incidents and how they are handled can be very useful for preparing potential incident handling role holders. It also helps reveal issues with the incident handling policy, process, and procedures. However, these exercises require that OT staff take valuable time away from normal responsibilities. For this reason, this task is marked as optional for any given periodic incident response preparation process, but ought to occur at a minimum frequency of once every 3 years or after significant changes have been made to personnel within the OT staff.