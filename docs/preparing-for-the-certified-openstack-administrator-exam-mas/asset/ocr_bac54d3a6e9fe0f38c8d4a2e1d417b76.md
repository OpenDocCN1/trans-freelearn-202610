To restore this backup, we can use the **Restore Backup** button in the **Volume Backups** tab.

# CLI

Let's create a new Cinder backup via `python-openstackclient`.

Source the newly created `openrc-quality-control` file:

```
$ source openrc-quality-control
```

Verify that the proper variables are set:

```
$ export | grep OS
```

Create the backup:

```
$ openstack volume backup create --name volume-backup-cli --description "COA exam prep dashboard volume backup" --container volume-backup-container-cli volume-cli
```

After the backup completes, you should be able to confirm that the backup was successfully created:

```
$ openstack volume backup list
```

You should see output similar to Figure 7.22.

![](bbox=[0.02, 0.58, 0.92, 0.65])

Figure 7.22: Output from the 'openstack volume backup list' command

If you wanted to restore the backup on the CLI, you must manually create an empty volume first, then the run the following:

```
$ openstack volume backup restore volume-backup-cli <target-volume>
```

# Note

Before proceeding to the next chapter, be sure to delete all running instances to free available resources. You can run the following:

```
$ openstack server list --all-projects
$ openstack server delete <server-uuid>
```