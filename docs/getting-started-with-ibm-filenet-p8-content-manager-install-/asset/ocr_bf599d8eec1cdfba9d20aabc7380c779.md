# Bringing up the P8 Domain

On your first FEM connection to the new CE installation, you will be guided through a series of steps that we informally call Domain bring-up. It's the final phase of configuration that leads to a functional Domain. The very first step in that is the choice of a name for the Domain. The Domain name doesn't show up in many places because the use of a given CE connection URL implies a particular Domain. So, you can call it pretty much anything you want. For our examples, we'll call our Domain Lucky.

The next step is the wizard for creating a directory configuration. Based on how much time we spent with directory data in the Configuration Manager and the WAS admin console, you could probably type this information in your sleep. The earlier steps were done so that WAS could do authentication. The CE steps are done so that the CE can resolve group memberships for authorization and also so it can service user and group searches done via CE APIs. We won't belabor the attribute values again, but the main configuration panel looks as follows:

![](bbox=[0.25, 0.37, 0.72, 0.56])

The user directory configuration properties look like this:

![](bbox=[0.25, 0.6, 0.72, 0.75])