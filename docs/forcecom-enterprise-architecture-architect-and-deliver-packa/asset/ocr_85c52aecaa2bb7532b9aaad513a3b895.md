Once you are happy with the information on the screen, click on the **Upload** button once again to begin the packaging process. Once the upload process completes, you will see a confirmation page as follows:

![](bbox=[0.085, 0.125, 0.83, 0.365])

Packages can be uploaded in one of two states, as described here:

+   **Release:** Release packages can be installed into subscriber production orgs and can also provide an upgrade path from previous releases. The downside is that you cannot delete the previously released components, or change certain things, such as a field's type. Changes to the components that are marked **global**, such as Apex code and Visualforce components, are also restricted. While Salesforce is gradually enhancing the platform to provide the ability to modify certain released aspects, you need to be certain that your application release is stable before selecting this option.

+   **Beta:** Beta packages cannot be installed into subscriber production orgs; you can install only into Developer Edition (such as your testing org), **sandbox**, or Partner Portal created orgs. Also, Beta packages cannot be upgraded once installed; this is the reason why Salesforce does not permit their installation into production orgs. The key benefit is in the ability to continue to change new components of the release to address bugs and features relating to user feedback.

> The ability to delete previously-published components (uploaded within a release package) is, at the time of writing this book, in pilot. It can be enabled by raising a support case with Salesforce Support. Once you have understood the full implications, they will enable it.