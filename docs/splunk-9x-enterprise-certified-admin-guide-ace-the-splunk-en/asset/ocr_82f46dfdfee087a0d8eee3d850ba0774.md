The precedence of .conf files in different directories determines which settings take priority. Let’s explore the order in which these configuration files are considered.

# Understanding conf file precedence

$SPLUNK_HOME/etc is the parent directory where all conf files exist in a typical Splunk Enterprise installation. A configuration file can be created in more than one place under this parent directory. You might be wondering why there is a need to have the same type of file in multiple places.

The ability to configure settings at various levels within Splunk provides flexibility to administrators, developers, and users to customize the platform according to their specific needs. This flexibility allows for precise control over individual apps and user experiences. The precedence of files that have the same stanza names in multiple directories is determined by Splunk. Precedence is covered later in this section.

By default, Splunk Enterprise ships all the system-wide configurations under the $SPLUNK_HOME/etc/system/default directory, which is not supposed to be altered. So, it is suggested that if a change to a conf file under the /default directory is required, create it under the $SPLUNK_HOME/etc/system/local directory, which takes precedence over the /default directory. Similarly, conf files could be present under the following directories:

+   - $SPLUNK_HOME/etc/users: This directory contains private, user-specific changes that are owned by each individual user. This ownership ensures that user-specific changes are isolated and not shared with other users by default.
+   - $SPLUNK_HOME/etc/apps/<app_name>/[default|local]: Configurations created within an app’s <app_name> directory are stored here. These configurations are available to any user who has access to the app. By assigning specific roles to users, you can control their access to apps, including the ability to view and modify the configurations within the <app_name> directory.
+   - $SPLUNK_HOME/etc/system/local: The configurations stored in this Splunk directory are considered global configurations. These configurations are accessible to every user in the Splunk instance.

At this stage, you are aware of the list of configuration file directories. Let’s understand how Splunk decides the precedence of the same conf files in different directories (refer to Figure 6.1). The precedence is decided based on the conf type input, props, indexes, and so on, and their context.

The context in which a configuration setting is used can also affect the order of precedence. For example, some configuration settings may only apply to a specific app or data source, while others may apply globally to the entire Splunk environment. The two types of context are as follows:

+   - **Global**: Configuration files that are independent of user and app contexts include settings related to indexing, monitoring, deployment, server configurations, and so on. These configurations are independent of user and app context and apply globally to all users in the Splunk instance.