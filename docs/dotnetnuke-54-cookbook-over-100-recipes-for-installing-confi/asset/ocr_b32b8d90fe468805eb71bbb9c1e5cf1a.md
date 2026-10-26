# How to do it...

+   1. Log in as SuperUser (`syshost` in our examples).
+   2. Look under the **Admin** menu and select **Languages.** The installed language packs for your portal are shown.
+   3. Click on the edit icon (the little pencil) next to Français (Canada) under Portal to enter the **Language Editor.**

![](bbox=[0.12, 0.245, 0.88, 0.41])

+   4. Click on **LocalResources** on the left. Expand the subfolder and drill down until you see EditEmployee.ascx.
+   5. Click on EditEmployee.ascx to display the available translation strings.
+   6. If this is the first time you are creating French language translations for this module, you will at first see English translations taken from the default language resource file we created back in the recipe *Displaying labels from the resource file*.
+   7. Substitute each English string for a French translation string as follows:
+      - `ControlTitle_edit.Text`: **Modifier Employé**
+      - `lblEmpFirstName.Help`: **Entrez le prénom de l'employé.**
+      - `lblEmpFirstName.Text`: **Prénom**
+      - `lblEmpLastName.Help`: **Entrez le nom de famille de l'employé.**
+      - `lblEmpLastName.Text`: **Nom de famille**
+      - `lblHireDate.Help`: **Entrez la date d'embauche de l'employé.**
+      - `lblHireDate.Text`: **Date d'embauche**
+      - `lblManagerNo.Help`: **Entrez le numéro d'identification du gérant salarié**
+      - `lblManagerNo.Text`: **Gestionnaire #**
+      - `lblSalary.Help`: **Entrez salaire annuel du salarié.**
+      - `lblSalary.Text`: **Salaire**