# Eradication procedure

The primary goal of the eradication phase is to get rid of the affected assets of security incident artifacts. Using information gathered during the identification and containment steps, determine a root cause and incident path. Then, if possible, prevent the root cause of the incident from recurring anywhere in the OT environment or, at least, establish an alert when similar incident symptoms occur in the future. It is not necessary to identify an attacking host (by name, location, or IP address), and attempts to do so should not distract from eradication and recovery.

Vulnerability analysis and improving defenses typically occur during the eradication phase. While the focus is on identifying the vulnerability that allowed this incident to occur, broad vulnerability analysis is generally appropriate if it can be executed without delaying progress toward recovery. Improving defenses at this stage should be limited to those mitigations that can be implemented quickly and relatively easily, such as disabling unused services and/or installing security patches throughout the OT environment. Other improvements and mitigations should be addressed during the "lessons learned" step, to keep the focus on recovering from this security incident.

Eradication can often be achieved by doing one of the following:

+   - Removing malicious software
+   - Disabling a breached user account
+   - Restoring the asset from a clean backup
+   - Rebuilding the asset from source media

Eradication actions should be documented including what was done to which OT asset, when, and by whom in a notebook.

It is important that you don't proceed to the recovery phase until a root cause has been identified for this security incident and the scope is understood; otherwise, there is a significant risk of this or a related security incident occurring again. For example, if a rootkit is involved, system analysis tools may report unreliable information, so restoring from a clean backup or rebuilding from source media is required before the asset can return to normal operations.