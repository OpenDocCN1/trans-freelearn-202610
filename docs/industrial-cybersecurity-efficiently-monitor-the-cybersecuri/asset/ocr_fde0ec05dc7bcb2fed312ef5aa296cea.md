+   - How many assets are involved?
+     - If the number is larger than a couple or a few, it is usually best to act quickly to isolate assets until further analysis can be performed.

Typically, the short-term containment task action that’s performed for each affected asset is one of the following:

+   - Isolate the asset from the network (wired and wireless, if applicable).
+   - Shut down the asset’s power (using shutdown practices that are gentle to the asset’s applications and databases, if possible).
+   - Disable functions (such as data replication) that directly impact other assets and disable all remote access.
+   - Allow the asset to operate as normal for the short term, with increased oversight.

Short-term containment is intended to involve temporary measures aimed at limiting damage and buying time necessary to do incident handling planning and severity determination. Containment actions should be documented as to what was done to which OT asset, when, and by whom in a notebook.

# ## Long-term containment procedure

Long-term containment picks up where short-term containment leaves off, and includes the following tasks:

+   - Verifying the nature of the incident.
+   - Verifying the scope of the incident.
+   - Determining the minimum change necessary to stop the incident.
+   - Collecting data that shows evidence of the incident occurring, as well as evidence that the symptoms of the incident are no longer present.
+   - For high- and some medium-severity incidents, take a backup image of the affected system if practical (known as a forensic image), or a copy of the files on the hard drives (known as a forensic copy).
+   - Undoing short-term containment actions that are more than the minimum change necessary.
+   - Verifying that incident symptoms have ceased occurring.