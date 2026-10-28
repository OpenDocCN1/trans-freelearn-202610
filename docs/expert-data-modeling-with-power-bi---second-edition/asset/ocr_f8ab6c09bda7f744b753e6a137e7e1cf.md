is important to note that either RLS or OLS kicks in only at the report level and only if:

+   - The user has the **Viewer** role in the Workspace.
+   - The report is shared with the user via a **link**, **direct access**, or an **app**.

Note
As mentioned earlier, RLS filters data across an entire data model, and OLS hides the data model objects from unauthorized users on the report layer. So, they are not security or permission control configurations over Power BI data models.

# Terminology

We should learn some terminology before implementing RLS and OLS. The following sections introduce this terminology.

# Roles

A role is a name indicating the characteristic of a security rule over tables in the data model. It is best to pick a meaningful name describing the underlying security rules. We can define roles in Power BI Desktop as follows:

+   1. Click the **Modeling** tab.
+   2. Click the **Manage roles** button.
+   3. Click the **Create** button to add a new role.

The preceding steps are highlighted in the following image: