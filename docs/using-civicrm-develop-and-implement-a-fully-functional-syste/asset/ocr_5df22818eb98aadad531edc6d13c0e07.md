+   - **File:** Browse button to upload a file. Files will be stored in the upload directory defined in global settings. A link to access the file will be presented in the custom data display. When you return to edit a record for which an existing file has been attached you will have the option of replacing the file with another upload or deleting the existing file.
+   - **Link:** Hyperlink, which will be hot-linked when in view mode.
+   - **Contact Reference:** Used to link to another contact record. The field will render as an auto complete field and will allow you to begin typing to locate another contact in the database.

> > By default, the contact reference type field will pull contact matches from the entire database. In most cases, you will not want to do this, as it would expose all your contact records to whoever is using the form. CiviCRM provides a hook for customizing the list of records available to the auto complete field: `hook_civicrm_contactListQuery`. For more details about using hooks to customize CiviCRM, visit `http://wiki.civicrm.org/confluence/display/CRMDOC/CiviCRM+hook+specification`.

After selecting your field type, choose your preferred display format. The options available depend on the field type, and several field types have only one option value. We will list and describe all of the display types that may be available.

When choosing a display type that includes a list of options (such as select / checkbox / radio buttons), you will be provided a form for creating the option labels and values. You can enter up to 10 options initially; after saving them, you can add more. You are also given the option of reusing a list previously created for another field. The label is what will appear in the forms; the value is what will be recorded in the database and exported from the system.

> > Label text should be descriptive and meaningful to the person completing the form. Most of the times you will enter the same value for both label and value, unless you have a good reason to make them different.

The display types that appear are as follows:

+   - **Text:** Basic text field.
+   - **Select:** Drop-down box; select one option.
+   - **Radio:** Radio buttons allow you to select one option from the list available. If the field is not required, a link to unselect any options will be included in the display. You may decide how many options will be listed per line.