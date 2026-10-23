# Sandbox refreshes

Probably the easiest way to back up metadata from your production Salesforce instance is by initiating a sandbox refresh. This facility either creates or refreshes a designated sandbox environment with a copy of the production metadata (but depending on the sandbox environment type, data may or may not be present).

With sandbox refreshes covered, let’s investigate change sets.

# Change sets

Change sets are often the first way that many Salesforce administrators and developers learn to move selective metadata changes from a sandbox environment to the production environment. Change sets are non-developer friendly and essentially act as the de facto way for non-developers to move sets of metadata between Salesforce environments. Whilst most change set usage is for moving changes up into the production environment, change sets can also be used for moving changes from the production environment to a specific sandbox environment.

Having understood how change sets are used for selective metadata movement between environments, let’s touch upon Salesforce Developer Experience (SFDX).

# SFDX

A developer-focused suite of command-line utilities, SFDX allows for metadata changes to be exported and imported from the production environment or any sandbox created from the production environment to which the user has login access. Knowledge of the specifics of SFDX usage is not important for the exam but it is useful to know that it can be used by developers to export metadata from Salesforce.

# The Ant Migration Tool

Much like SFDX, the Ant Migration Tool is another command-line utility for moving your Salesforce metadata between a local directory and a Salesforce instance. Again, as with SFDX, the Ant Migration Tool can be used to move Salesforce metadata from a production environment to a local directory.

Now that we’ve explored backing up metadata, let’s move on to importing metadata.

# Importing metadata

As you can probably imagine by now, all methods besides the sandbox refresh process can be used to import metadata as a way of restoring the metadata to a Salesforce instance. Let’s start with change sets.

# Change sets

By capturing a selected set of metadata artifacts and moving them from one Salesforce environment to another, change sets can be used to restore a set of changes from one environment to another. Imagine a Salesforce environment refreshed from production by accident – if there were another Salesforce