This also applies to some attributes of components that are flagged as upgradeable in the table referenced previously, but have certain attributes that are then not upgradeable. Refer to the Editing Components and Attributes After Installation section in ISVforce Guide for the latest details on this. Some common attributes that are packable but not upgradable, and hence, end user modifiable are as follows:

+   - Custom field: Picklist values
+   - Validation Rule: Active
+   - Custom Object: Action overrides

> The active status of the Validation Rule is quite a surprising one; while the user cannot modify your packaged Validation Rule, they can disable it. For most purposes, this means that this platform feature becomes a post-install optional activity left to the end user where they should wish to implement rules unique to them. For validations you wish to implement in Apex, you can utilize a protected Custom Setting to control activation if desired (such settings are only visible to your support teams via subscriber support).

# Custom field – picklist values

Probably the most surprising of the non-upgradable attributes of the custom field is the list of picklist values. These are completely modifiable by end users; they can be added, modified, or deleted. This means that you need to be very careful about how you reference these in your Apex code, and consequently, you provide guidelines to your end users about editing and applying changes post upgrade.

> The Salesforce Translation Workbench tool, under the Setup menu, can be used to effectively relabel a pick list value by editing the users' current language without having to rename its value on the field definition. This can be done after installation of the tool by the end user admin.

Also, remember that the platform treats picklist fields mostly as if they were text fields; as such, you can query and even update a picklist field with any value you like. Also, keep in mind that you can upgrade customer data (if you change or delete a picklist value) through an Apex post-install script.