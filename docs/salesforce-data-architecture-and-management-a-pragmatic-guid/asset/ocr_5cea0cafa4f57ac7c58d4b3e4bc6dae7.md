+   • **Full-copy sandbox**: When you are ready for UAT and to have the business testing team validate your setup/code, ensure that you have a full-copy sandbox set up for this purpose. Running a full migration in this type of sandbox will give some very good data, for example, how long it would take to run the migration in the production environment. This is because full-copy sandboxes have similar (not the same) hardware and results extrapolated are typically valid for production as well.

Another benefit is that full-copy sandboxes have the same data as a production environment, so you are working with an environment that very closely resembles a production environment. Of course, if this is a greenfield implementation, then the production environment will not have any data, but in the case of a brownfield implementation, this can be an advantage.

To re-iterate, sandbox testing is a must regardless of how simple or complex the data migration is. For simpler migrations, it seems like an extraneous step, but I think it’s worth the time and the effort rather than going back and trying to restore data in the production org in the case of a brownfield org.

Let’s now review best practices related to the production run.

# Production run

You have conducted multiple dry runs and had business testing validate your results in a full cycle UAT. It’s time to press the trigger and run the data migration in the production environment. Some of the best practices for this phase include the following:

+   • **Production freeze**: Ensure that you have communicated to other stakeholders that there should be a change freeze for metadata in the production environment. This is to avoid running into any issues due to enhancements that were not applied to the sandbox environments. Break-fixes cannot be avoided but if there are any that are not a high priority, it is recommended to postpone them until you have completely migrated your data.

A change freeze should usually kick in when you move to the full-copy environment and are conducting UAT testing there. To avoid holding up other projects or teams, try to minimize the time between the change freeze and the production run of your migration. I should mention that for simpler migrations, such extensive measures may not be needed but nonetheless, it’s a good practice to follow to avoid any unexpected issues.